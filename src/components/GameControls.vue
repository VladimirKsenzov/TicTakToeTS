<script setup lang="ts">
import type { CellValue } from '../types';

defineProps<{
  firstPlayer: CellValue;
}>();

const emit = defineEmits<{
  (event: 'select-first-player', value: CellValue): void;
  (event: 'restart'): void;
}>();

function handleSelect(event: Event) {
  const value = (event.target as HTMLSelectElement).value as CellValue;
  emit('select-first-player', value);
}

function handleRestart() {
  emit('restart');
}
</script>

<template>
  <div class="game-controls">
    <p class="game-controls__text">Первый игрок: {{ firstPlayer }}</p>
    <select
      class="game-controls__select"
      :value="firstPlayer"
      @change="handleSelect"
    >
      <option value="X">X</option>
      <option value="O">O</option>
    </select>
    <button @click="handleRestart" class="game-controls__button">
      Начать заново
    </button>
  </div>
</template>

<style scoped>
.game-controls {
  margin-top: 20px;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
}

.game-controls__text {
  font-weight: bold;
  font-size: 1rem;
}

.game-controls__select {
  padding: 5px;
  font-size: 1rem;
}

.game-controls__button {
  padding: 6px 12px;
  background-color: #2c3e50;
  color: #fff;
  border: none;
  cursor: pointer;
  font-size: 1rem;
  border-radius: 4px;
  transition: background-color 0.3s;
}

.game-controls__button:hover {
  background-color: #1a252f;
}
</style>
