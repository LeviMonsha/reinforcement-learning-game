<template>
  <button class="text-white" @click="clearCanvas">
    <BeakerIcon class="size-6 text-white" />
  </button>
  <canvas
    ref="canvas"
    @mousedown="startDrawing"
    @mouseup="stopDrawing"
    @mousemove="draw"
    :width="width"
    :height="height"
    class="relative"
    style="border: 1px solid #000"
  />
  <slot />
</template>

<script setup>
import { ref, onMounted } from "vue";
import { BeakerIcon } from "@heroicons/vue/24/solid";

// eslint-disable-next-line no-undef, no-unused-vars
const props = defineProps(["width", "height", "isDrawingAllowed"]);
// eslint-disable-next-line no-undef, no-unused-vars
const emit = defineEmits(["lineDrawn"]);
const canvas = ref(null);
const isDrawing = ref(false);
const context = ref(null);
const lastX = ref(0);
const lastY = ref(0);

onMounted(() => {
  context.value = canvas.value.getContext("2d");
});

function startDrawing(event) {
  // eslint-disable-next-line no-undef
  if (!props.isDrawingAllowed) return;

  isDrawing.value = true;
  [lastX.value, lastY.value] = [event.offsetX, event.offsetY];
}

function stopDrawing() {
  isDrawing.value = false;
  context.value.beginPath();
}

function draw(event) {
  // eslint-disable-next-line no-undef
  if (!isDrawing.value || !props.isDrawingAllowed) return;

  context.value.strokeStyle = "white";
  context.value.lineWidth = 8;
  context.value.beginPath();
  context.value.moveTo(lastX.value, lastY.value);
  context.value.lineTo(event.offsetX, event.offsetY);
  context.value.stroke();

  [lastX.value, lastY.value] = [event.offsetX, event.offsetY];
}

function clearCanvas() {
  if (!props.isDrawingAllowed) return;
  context.value.clearRect(0, 0, canvas.value.width, canvas.value.height);
}
</script>

<style scoped>
canvas {
  cursor: crosshair;
}
</style>
