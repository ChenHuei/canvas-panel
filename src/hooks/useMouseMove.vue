<script lang="ts">
import { ref, onMounted, onUnmounted } from "vue";

export const useMouseMove = () => {

  const x = ref(0);
  const y = ref(0);

  const move = (e: MouseEvent): void => {
    x.value = e.clientX;
    y.value = e.clientY;
  };

  onMounted(() => {
    window.addEventListener("mousemove", move);
  });

  onUnmounted(() => {
    window.removeEventListener("mousemove", move);
  });

  return { x, y };
};
</script>
