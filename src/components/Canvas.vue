<template>
  <canvas id="canvas" width="0" height="0" />
</template>

<script lang="ts">
import { defineComponent, ref, onMounted, Ref } from "vue";

// hooks
import { useMouseMove } from "../hooks/useMouseMove.vue";

export default defineComponent({
  setup() {
    const { x, y } = useMouseMove();

    const canvas: Ref<null | HTMLCanvasElement> = ref(null);
    const ctx: Ref<null | CanvasRenderingContext2D> = ref(null);

    onMounted(() => {
      canvas.value = document.querySelector("#canvas") as HTMLCanvasElement;
      ctx.value = canvas.value.getContext("2d") as CanvasRenderingContext2D;
    });

    return {
      canvas,
      ctx,
      x,
      y,
    };
  },
  methods: {
    initCanvas(): void {
      if (this.ctx !== null) {
        this.ctx.lineCap = "round";
        this.ctx.lineJoin = "round";
        this.ctx.strokeStyle = "black";
        this.ctx.lineWidth = 1;
      }
    },
    draw(x: number, y: number): void {
      if (this.ctx !== null) {
        this.ctx.beginPath();
        this.ctx.moveTo(x, y);
        this.ctx.lineTo(x + 1, y + 1);
        this.ctx.stroke();
      }
    },
  },
});
</script>
