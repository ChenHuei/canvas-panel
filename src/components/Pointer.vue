<template>
  <div
    ref="pointer"
    class="pointer absolute rounded-full bg-black transition pointer-events-none"
    :class="{ hidden: isHidden }"
    :style="styleHandler"
  ></div>
</template>

<script lang="ts">
import { defineComponent, ref, onMounted, Ref, computed } from "vue";

export default defineComponent({
  props: {
    x: {
      type: Number,
      required: true,
    },
    y: {
      type: Number,
      required: true,
    },
    isHidden: {
      type: Boolean,
      default: false,
    },
  },
  setup(props) {
    const pointer: Ref<null | Element> = ref(null);

    let clientWidth = 0;
    let clientHeight = 0;

    const styleHandler = computed(() => {
      return {
        left: `${props.x - clientWidth / 2}px`,
        top: `${props.y - clientHeight / 2}px`,
      };
    });

    onMounted(() => {
      if (pointer.value) {
        clientWidth = pointer.value.clientWidth;
        clientHeight = pointer.value.clientHeight;
      }
    });

    return {
      pointer,
      styleHandler,
    };
  },
});
</script>

<style lang="scss" scoped>
.pointer {
  width: 16px;
  height: 16px;
}
</style>
