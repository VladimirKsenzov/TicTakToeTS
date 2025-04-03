<template>
  <div
    class="cell"
    :class="{
      'cell--winner': winner,
      'cell--disabled': isDisabled,
    }"
    @click="handleClick"
  >
    {{ value }}
  </div>
</template>

<script setup lang="ts">
import { defineProps, defineEmits } from 'vue';

interface Props {
  value: string | null;
  winner: boolean;
  isDisabled: boolean;
}

interface Emits {
  (event: 'click'): void;
}

const { value, winner, isDisabled } = defineProps<Props>();
const emit = defineEmits<Emits>();

function handleClick(): void {
  if (isDisabled) return;
  emit('click');
}
</script>

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

.cell--disabled {
  pointer-events: none;
  cursor: default;
}

.cell--winner {
  background-color: #a6f5b9;
}
</style>
