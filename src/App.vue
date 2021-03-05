<template>
  <main class="w-screen h-screen bg-gray-200">
    <Header
      @save="saveHandler"
      @clear="clearHandler"
      @undo="undoHandler"
      @redo="redoHandler"
    />
    <Canvas />
  </main>
</template>

<script lang="ts">
import { defineComponent } from "vue";

// Components
import Header from "./components/Header.vue";
import Canvas from "./components/Canvas.vue";

export default defineComponent({
  name: "App",
  components: { Header, Canvas },
  methods: {
    saveHandler(): void {
      console.log("saveHandler");
    },
    clearHandler(): void {
      console.log("clearHandler");
    },
    undoHandler(): void {
      console.log("undoHandler");
    },
    redoHandler(): void {
      console.log("redoHandler");
    },
  },
});
</script>

<style lang="scss">
:root  {
  --primary-light: '#9BFFCD',
  --primary: '#00CC99'
  --primary-dark: '#01936F'
  --gray: '#E8E8E8'
  --white: '#FFFFFF',
  --black: '#000000'
}
</style>
