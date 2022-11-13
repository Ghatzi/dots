<script setup lang="ts">
import { ref } from "vue";

type TPoint = {
  x: number;
  y: number;
};

const points = ref<TPoint[]>([]);
const popped = ref<TPoint[]>([]);

const handleClick = (e: MouseEvent) => {
  const { clientX, clientY } = e;

  points.value = [
    ...points.value,
    {
      x: clientX,
      y: clientY,
    },
  ];
};

const handleUndo = (e: Event) => {
  e.stopPropagation();
  const newPoints = [...points.value];
  const poppedPoint = newPoints.pop();
  if (!poppedPoint) return;
  popped.value = [...popped.value, poppedPoint];
  points.value = newPoints;
};

const handleRedo = (e: Event) => {
  e.stopPropagation();
  const newPopped = [...popped.value];
  const poppedPoint = newPopped.pop();
  if (!poppedPoint) return;
  points.value = [...points.value, poppedPoint];
  popped.value = newPopped;
};
</script>

<template>
  <div class="container" @click="handleClick">
    <div class="btn-container">
      <button :disabled="points.length === 0" @click="handleUndo">UNDO</button>
      <button :disabled="popped.length === 0" @click="handleRedo">REDO</button>
    </div>

    <div
      v-for="(point, idx) in points"
      :key="idx"
      class="points"
      :style="{ left: point.x - 5 + 'px', top: point.y - 5 + 'px' }"
    ></div>
  </div>
</template>

<style scoped>
.container {
  height: 100vh;
  border: 1px solid #f5f5f5;
}

.container .points {
  display: inline-block;
  position: absolute;
  border-radius: 50%;
  width: 10px;
  height: 10px;
  background: #af1685;
}

.btn-container {
  display: flex;
  justify-content: center;
  gap: 10px;
  margin: 10px;
}

.btn-container button {
  background: #af1685;
  border: 2px solid #af1685;
  color: white;
  border-radius: 4px;
  outline: none;
  transition: all 0.5s ease;
  padding: 8px 32px;
  display: block;
  cursor: pointer;
}

.btn-container button:hover {
  background: #ea56c1;
}

.btn-container button:disabled {
  background: #bfbfbf;
  border: 2px solid #bfbfbf;
  cursor: not-allowed;
}
</style>
