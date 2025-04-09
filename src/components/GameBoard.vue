<script setup lang="ts">
import Cell from './Cell.vue';
import type { CellValue } from '../types';

defineProps<{
  board: CellValue[];
  winningCombination: number[];
  isDisabled: boolean;
}>();
</script>

<template>
  <div class="game-board" v-on="$attrs">
    <Cell
      v-for="(cell, index) in board"
      :key="index"
      :value="cell"
      :index="index"
      :isDisabled="isDisabled"
      :winner="winningCombination.includes(index)"
    />
  </div>
</template>

<style scoped>
.game-board {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
}
</style>
