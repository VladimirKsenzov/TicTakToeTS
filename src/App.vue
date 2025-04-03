<template>
  <div class="app">
    <GameStatus :status="status" />
    <GameBoard
      :board="board"
      :winningCombination="winningCombination"
      @cell-click="handleCellClick"
      :isDisabled="isGameOver"
    />
    <GameControls v-model="firstPlayer" @restart="restartGame" />
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue';
import GameBoard from './components/GameBoard.vue';
import GameStatus from './components/GameStatus.vue';
import GameControls from './components/GameControls.vue';

type CellValue = 'X' | 'O' | null;

const board = ref<CellValue[]>(Array(9).fill(null));
const currentPlayer = ref<'X' | 'O'>('X');
const status = ref<string>('Ход: X');
const firstPlayer = ref<'X' | 'O'>('X');
const winningCombination = ref<number[]>([]);

function getWinningCombination(): number[] | null {
  const winPatterns: number[][] = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];

  for (const pattern of winPatterns) {
    const [a, b, c] = pattern;
    if (
      board.value[a] &&
      board.value[a] === board.value[b] &&
      board.value[a] === board.value[c]
    ) {
      return pattern;
    }
  }
  return null;
}

function handleCellClick(index: number): void {
  if (board.value[index] || winningCombination.value.length > 0) return;

  board.value[index] = currentPlayer.value;

  const winCombo = getWinningCombination();
  if (winCombo) {
    winningCombination.value = winCombo;
    status.value = `Победитель: ${currentPlayer.value}`;
  } else if (board.value.every((cell) => cell)) {
    status.value = 'Ничья';
  } else {
    currentPlayer.value = currentPlayer.value === 'X' ? 'O' : 'X';
    status.value = `Ход: ${currentPlayer.value}`;
  }
}

function restartGame(): void {
  board.value = Array(9).fill(null);
  currentPlayer.value = firstPlayer.value;
  status.value = `Ход: ${currentPlayer.value}`;
  winningCombination.value = [];
}

const isGameOver = computed(() => {
  return (
    winningCombination.value.length > 0 || board.value.every((cell) => cell)
  );
});
</script>

<style scoped>
.app {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
}
</style>
