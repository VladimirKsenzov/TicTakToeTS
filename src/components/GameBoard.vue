<template>
  <div class="game-board">
    <Cell
      v-for="(cell, index) in board"
      :key="index"
      :value="cell"
      :winner="winningCombination.includes(index)"
      @click="cellClicked(index)"
      class="game-board__cell"
      :isDisabled="isDisabled"
    />
  </div>
</template>

<script setup lang="ts">
import { defineProps, defineEmits } from 'vue';
import Cell from './Cell.vue';

interface Props {
  board: (string | null)[];
  winningCombination: number[];
  isDisabled: boolean;
}

const { board, winningCombination, isDisabled } = defineProps<Props>();

const emit = defineEmits<{
  (event: 'cell-click', index: number): void;
}>();

function cellClicked(index: number): void {
  emit('cell-click', index);
}
</script>

<style scoped>
.game-board {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
}
</style>
