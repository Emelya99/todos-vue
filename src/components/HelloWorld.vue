<script setup lang="ts">
import { ref } from 'vue'

interface Props {
  msg: string;
}

defineProps<Props>()

const count = ref(0)

const increment = (): void => {
  count.value++
}

const decrement = (): void => {
  if (count.value > 0) {
    count.value--
  }
}
</script>

<template>
  <h1>{{ msg }}</h1>

  <h2>Count is {{ count }}</h2>

  <button type="button" @click="increment">+1</button>
  <button type="button" @click="decrement">-1</button>
</template>

<style scoped>

</style>
