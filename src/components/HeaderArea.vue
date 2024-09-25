<script setup lang="ts">
const { title, quizStart, quizStatus } = defineProps({
  title: String,
  quizStatus: String,
  quizStart: Boolean
})
</script>

<template>
  <header class="container flex justify-center items-center flex-col h-[25%] py-6">
    <h1 v-if="quizStart" class="text-primary text-4xl text-center">{{ quizStatus }}</h1>
    <h2 class="text-accent text-2xl leading-6 py-2">{{ title }}</h2>
  </header>
</template>

<style scoped></style>
