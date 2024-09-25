<script lang="ts" setup>
const { quizScore, totalScore } = defineProps<{
  quizScore: number
  totalScore: number
}>()
const handleReload = () => {
  window.location.reload()
}
</script>

<template>
  <div class="container flex justify-center items-center w-full h-[80vh] flex-col">
    <img
      class="size-52"
      src="../assets/confetti-7951019_1280.png"
      alt="Image by Thor Deichmann from Pixabay"
    />
    <h1 class="text-lime-800 text-4xl text-center">Congratulation. You have completed your Quiz</h1>
    <h3 class="italic text-2xl text-red-600 pt-5">
      Your Score is {{ quizScore }} out of {{ totalScore }}
    </h3>
    <button class="btn btn-secondary mt-10" @click="handleReload()">Restart Quiz</button>
  </div>
</template>

<style lang="scss" scoped></style>
