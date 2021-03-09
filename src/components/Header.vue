<template>
  <header
    class="header relative w-screen h-20 flex justify-center items-center bg-white transition-transform"
    :class="collapseClass"
  >
    <div
      v-for="item in list"
      :key="item.event"
      class="header-item flex items-center mr-8 transition-opacity cursor-pointer hover:opacity-40"
      :class="disabledClassHandler(item.disabledKey)"
      @click="$emit(item.event)"
    >
      <i class="fas mr-2" :class="[`fa-${item.icon}`, item.size]"></i>
      <p class="uppercase">{{ item.label }}</p>
    </div>
    <!-- arrow -->
    <div
      class="header-collapse absolute left-1/2 top-full w-12 h-12 flex justify-center items-center bg-white rounded-full transition cursor-pointer"
      @click="collapseHandler"
    >
      <i
        class="header-collapse-icon fas fa-chevron-up transition-transform"
        :class="collapseClass"
      ></i>
    </div>
  </header>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";

// Constants
import { HEADER_LIST } from "../constants/header";

export default defineComponent({
  props: {
    isDisabledSave: {
      type: Boolean,
      default: false,
    },
    isDisabledUndo: {
      type: Boolean,
      default: false,
    },
    isDisabledRedo: {
      type: Boolean,
      default: false,
    },
  },
  emits: ["save", "clear", "undo", "redo"],
  setup(props) {
    const isCollapse = ref(false);

    function collapseHandler(): void {
      isCollapse.value = !isCollapse.value;
    }

    function disabledClassHandler(
      key: "isDisabledSave" | "isDisabledUndo" | "isDisabledRedo"
    ) {
      return {
        disabled: key ? props[key] : false,
      };
    }

    return {
      ...props,
      list: HEADER_LIST,
      collapseHandler,
      collapseClass: {
        collapse: isCollapse.value,
      },
      disabledClassHandler,
    };
  },
});
</script>

<style lang="scss" scoped>
.header {
  &.collapse {
    transform: translateY(-100%);
  }

  &-collapse {
    transform: translate(-50%, -50%);

    &-icon {
      margin-top: 50%;
      transform: translateY(-50%);

      &.collapse {
        transform: rotate(180deg);
      }
    }
  }

  &-item {
    &.disabled {
      opacity: 0.4;
      cursor: not-allowed !important;
      pointer-events: none;

      &:hover {
        opacity: 0.4;
      }
    }
  }
}
</style>
