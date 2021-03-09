<template>
  <main class="w-screen h-screen relative bg-gray-200">
    <canvas
      class="absolute inset-0"
      ref="canvas"
      width="0"
      height="0"
      @mousedown="onMouseDown"
      @mouseup="onMouseUp"
      @mousemove="onMouseMove"
      @mouseleave="onMouseLeave"
      @mouseenter="onMouseEnter"
    />
    <Header
      :is-disabled-save="isDisabledSave"
      :is-disabled-undo="isDisabledUndo"
      :is-disabled-redo="isDisabledRedo"
      @save="saveHandler"
      @clear="clearHandler"
      @undo="undoHandler"
      @redo="redoHandler"
    />
    <Pointer v-bind="pointer" :is-hidden="isHiddenPointer" />
  </main>
</template>

<script lang="ts">
import { defineComponent, ref, reactive, onMounted, Ref, computed } from "vue";

// Components
import Header from "./components/Header.vue";
import Pointer from "./components/Pointer.vue";

export default defineComponent({
  name: "App",
  components: { Header, Pointer },
  setup() {
    // coordinate

    const isMouseDown = ref(false);
    const x = ref(0);
    const y = ref(0);
    const base64 = ref("");

    // history

    const current = ref(0);
    const history: string[] = reactive([]);

    // pointer

    const isHiddenPointer = ref(false);
    const pointer: { x: number; y: number } = reactive({ x: 0, y: 0 });

    // refs

    const canvas: Ref<null | HTMLCanvasElement> = ref(null);
    const context: Ref<null | CanvasRenderingContext2D> = ref(null);

    //computed

    const isDisabledSave = computed(
      () => history.length === 0 || current.value === 0
    );
    const isDisabledUndo = computed(() => current.value === 0);
    const isDisabledRedo = computed(() => current.value === history.length);

    // function

    // init

    function init(): void {
      if (canvas.value) {
        canvas.value.width = window.innerWidth;
        canvas.value.height = window.innerHeight;

        context.value = canvas.value.getContext(
          "2d"
        ) as CanvasRenderingContext2D;
        context.value.clearRect(0, 0, canvas.value.width, canvas.value.height);
        context.value.lineCap = "round";
        context.value.strokeStyle = "#000000";
        context.value.lineWidth = 16;
      }
    }

    // handler

    function saveHandler(): void {
      const link = document.createElement("a");
      link.href = base64.value;
      link.download = "file.png";
      document.body.appendChild(link);
      link.click();
      link.remove();
    }

    function clearHandler(): void {
      if (context.value && canvas.value) {
        context.value.clearRect(0, 0, canvas.value.width, canvas.value.height);
        current.value = 0;
        history.length = 0;
      }
    }

    function undoHandler(): void {
      if (isDisabledUndo.value) return;

      changeHistoryHandler(-1);
    }

    function redoHandler(): void {
      if (isDisabledRedo.value) return;

      changeHistoryHandler(1);
    }

    function changeHistoryHandler(step: 1 | -1): void {
      if (context.value && canvas.value) {
        current.value += step;
        context.value.clearRect(0, 0, canvas.value.width, canvas.value.height);

        if (current.value !== 0) {
          drawImageHandler(history[current.value - 1]);
        }
      }
    }

    function drawCanvasHandler(xx: number, yy: number): void {
      if (context.value) {
        context.value.beginPath();
        context.value.moveTo(x.value, y.value);
        context.value.lineTo(xx, yy);
        context.value.stroke();

        x.value = xx;
        y.value = yy;
      }
    }

    function drawImageHandler(src: string, x = 0, y = 0): void {
      const img = new Image();
      img.src = src;
      img.onload = () => {
        x = window.innerWidth / 2 - img.width / 2;
        y = window.innerHeight / 2 - img.height / 2;
        (context.value as CanvasRenderingContext2D).drawImage(img, x, y);
        base64.value = (canvas.value as HTMLCanvasElement).toDataURL();
      };
    }

    // listener

    function onMouseDown(event: MouseEvent): void {
      const { clientX, clientY } = event;

      isMouseDown.value = true;
      x.value = clientX;
      y.value = clientY;
    }

    function onMouseUp(): void {
      isMouseDown.value = false;

      base64.value = (canvas.value as HTMLCanvasElement).toDataURL();
      history.push(base64.value);
      current.value = history.length;
    }

    function onMouseMove(event: MouseEvent): void {
      const { clientX, clientY } = event;

      pointer.x = clientX;
      pointer.y = clientY;

      if (isMouseDown.value) {
        drawCanvasHandler(clientX, clientY);
      }
    }

    function onMouseLeave(): void {
      isMouseDown.value = false;
      isHiddenPointer.value = true;
    }

    function onMouseEnter(): void {
      isHiddenPointer.value = false;
    }

    onMounted(() => {
      init();
    });

    return {
      isMouseDown,
      x,
      y,
      base64,
      current,
      history,
      isHiddenPointer,
      pointer,
      canvas,
      context,
      isDisabledSave,
      isDisabledUndo,
      isDisabledRedo,
      saveHandler,
      clearHandler,
      undoHandler,
      redoHandler,
      onMouseDown,
      onMouseUp,
      onMouseMove,
      onMouseLeave,
      onMouseEnter,
    };
  },
});
</script>
