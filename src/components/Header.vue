<template>
  <header
    class="header relative w-screen h-20 flex justify-center items-center bg-white transition-transform"
    :class="{ collapse: isCollapse }"
  >
    <div
      v-for="item in list"
      :key="item.event"
      class="flex items-center mr-8 transition-opacity cursor-pointer hover:opacity-40"
      @click="$emit(item.event)"
    >
      <i class="fas mr-2" :class="[`fa-${item.icon}`, item.size]"></i>
      <h6 class="uppercase">{{ item.label }}</h6>
    </div>
    <div
      class="header-collapse absolute left-1/2 top-full w-12 h-12 flex justify-center items-center bg-white rounded-full transition cursor-pointer"
      @click="collapseHandler"
    >
      <i
        class="header-collapse-icon fas fa-chevron-up transition-transform"
        :class="{ collapse: isCollapse }"
      ></i>
    </div>
  </header>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";

// Constants
import { HEADER_LIST } from "../constants/header";

export default defineComponent({
  setup() {
    const isCollapseRef = ref(false);

    return {
      isCollapse: isCollapseRef,
      list: HEADER_LIST,
    };
  },
  methods: {
    collapseHandler(): void {
      this.isCollapse = !this.isCollapse;
    },
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
}
</style>
