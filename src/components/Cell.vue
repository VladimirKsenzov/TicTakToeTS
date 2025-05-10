<script setup lang="ts">
import type { CellValue } from '../types';

defineProps<{
  value: CellValue;
  isDisabled: boolean;
  winner: boolean;
  index: number;
}>();
</script>

<template>
  <button
    type="button"
    :disabled="isDisabled"
    class="cell"
    :class="{
      'cell--disabled': isDisabled,
      'cell--winner': winner,
    }"
    :data-index="index"
  >
    {{ value }}
  </button>
</template>

<style scoped>
.cell {
  width: 100px;
  height: 100px;
  border: 2px solid #000;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 42px;
  cursor: pointer;
  transition: ease 0.3s;
}
.cell:hover {
  background-color: #a6f5b9;
}
/* .cell--disabled {
  pointer-events: none;
  cursor: default;
  opacity: 0.5;
} */
.cell--winner {
  background-color: #a6f5b9;
}
</style>
