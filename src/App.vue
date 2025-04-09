<template>
  <div class="app">
    <GameStatus :status="status" />
    <GameBoard
      :board="board"
      :winningCombination="winningCombination"
      :isDisabled="isGameOver"
      @click="handleCellClick"
    />
    <GameControls
      :firstPlayer="firstPlayer"
      @select-first-player="updateFirstPlayer"
      @restart="restartGame"
    />
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import type { CellValue } from './types';
import { getWinningCombination } from './utilits/helpers';
import GameStatus from './components/GameStatus.vue';
import GameBoard from './components/GameBoard.vue';
import GameControls from './components/GameControls.vue';

const board = ref<CellValue[]>(Array(9).fill(null));

const winningCombination = ref<number[]>([]);

const isGameOver = ref<boolean>(false);

const status = ref<string>('Ход: X');

const firstPlayer = ref<CellValue>('X');

const currentPlayer = ref<CellValue>(firstPlayer.value);

function handleCellClick(event: MouseEvent): void {
  const target = event.target as HTMLElement;
  const indexStr = target.dataset.index;
  const index = indexStr ? Number(indexStr) : -1;
  if (index === -1) return;
  if (board.value[index] || isGameOver.value) return;

  board.value[index] = currentPlayer.value;

  const winCombo = getWinningCombination(board.value);
  if (winCombo) {
    winningCombination.value = winCombo;
    status.value = `Победитель: ${currentPlayer.value}`;
    isGameOver.value = true;
  } else if (board.value.every((cell) => cell !== null)) {
    status.value = 'Ничья';
    isGameOver.value = true;
  } else {
    currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X';
    status.value = `Ход: ${currentPlayer.value}`;
  }
}

function updateFirstPlayer(value: CellValue): void {
  if (value === null) {
    firstPlayer.value = 'X';
  } else {
    firstPlayer.value = value;
  }
}

function restartGame(): void {
  board.value = Array(9).fill(null);
  winningCombination.value = [];
  isGameOver.value = false;
  currentPlayer.value = firstPlayer.value;
  status.value = `Ход: ${currentPlayer.value}`;
}
</script>

<style scoped>
.app {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
  padding: 40px;
  font-family: Arial, sans-serif;
}
</style>
