<template>
  <div class="game-controls">
    <p class="game-controls__text">Первый игрок: {{ modelValue }}</p>
    <select v-model="localValue" class="game-controls__select">
      <option value="X">X</option>
      <option value="O">O</option>
    </select>
    <button @click="restart" class="game-controls__button">
      Начать заново
    </button>
  </div>
</template>

<script setup lang="ts">
import { ref, watch } from 'vue';
import { defineProps, defineEmits } from 'vue';
type Player = 'X' | 'O';

const props = defineProps<{
  modelValue: Player;
}>();

const emit = defineEmits<{
  (event: 'update:modelValue', value: Player): void;
  (event: 'restart'): void;
}>();

const localValue = ref<Player>(props.modelValue);

watch(localValue, (newVal) => {
  emit('update:modelValue', newVal);
});

function restart(): void {
  emit('restart');
}
</script>

<style scoped>
.game-controls {
  margin-top: 20px;
  display: flex;
  align-items: center;
  gap: 10px;
}
.game-controls__text {
  font-size: 1rem;
}
.game-controls__select {
  padding: 5px;
  font-size: 1rem;
}
.game-controls__button {
  padding: 5px 10px;
  font-size: 1rem;
  cursor: pointer;
}
</style>
