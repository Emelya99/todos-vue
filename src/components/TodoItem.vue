<script setup lang="ts">
import { defineProps } from 'vue';

interface Props {
  todo: Todo; // Используем интерфейс Todo для точной типизации свойства todo
}

const { todo } = defineProps<Props>();
</script>

<template>
  <li>
    {{ todo.text }}
  </li>
</template>

<style>
/* Добавьте свои стили здесь */
</style>