<template>
  <div class="game-status">
    <p class="game-status__text">{{ status }}</p>
  </div>
</template>

<script setup lang="ts">
import { defineProps } from 'vue';

interface Props {
  status: string;
}

const { status } = defineProps<Props>();
</script>

<style scoped>
.game-status {
  margin-bottom: 20px;
}
.game-status__text {
  font-size: 1.2rem;
  font-weight: bold;
}
</style>
