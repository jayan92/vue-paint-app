<template>
  <h2>{{ message }}</h2>

  <div class="color-picker">
    <div
      v-for="color of colors"
      class="color-box"
      :style="{ backgroundColor: color }"
      @click="changeColor(color)"
    ></div>
  </div>

  <canvas id="canvas" @mousedown="startPainting" @mouseup="finishedPainting" @mousemove="draw"></canvas>
  <a class="clear-button" @click.prevent="clearCanvas">Clear Canvas</a>
</template>

<script setup>
import { onMounted, ref } from "vue";

const message = ref("Drawing App");
const painting = ref(false);
const canvas = ref(null);
const ctx = ref(null);
const colors = ref(["#000000", "#FF0000", "#00FF00", "#0000FF", "#FFFF00", "#FF00FF", "#00FFFF"]);
const currentColor = ref("");

const changeColor = (color) => {
  ctx.value.strokeStyle = color;
  currentColor.value = color;
};

const clearCanvas = () => {
  ctx.value.clearRect(0, 0, canvas.value.width, canvas.value.height);
  currentColor.value = colors.value[0];
};

const startPainting = (e) => {
  painting.value = true;
  draw(e);
};

const finishedPainting = () => {
  painting.value = false;
  ctx.value.beginPath();
};

const draw = (e) => {
  if (!painting.value) return;

  ctx.value.lineWidth = 10;
  ctx.value.lineCap = "round";

  ctx.value.lineTo(e.clientX - canvas.value.offsetLeft, e.clientY - canvas.value.offsetTop);
  ctx.value.stroke();

  ctx.value.beginPath();
  ctx.value.moveTo(e.clientX - canvas.value.offsetLeft, e.clientY - canvas.value.offsetTop);
};

onMounted(() => {
  canvas.value = document.getElementById("canvas");
  ctx.value = canvas.value.getContext("2d");

  ctx.value.strokeStyle = colors.value[0];

  currentColor.value = colors.value[0];

  canvas.value.height = parseFloat(getComputedStyle(canvas.value).height);
  canvas.value.width = parseFloat(getComputedStyle(canvas.value).width);
});
</script>

<style scoped></style>
