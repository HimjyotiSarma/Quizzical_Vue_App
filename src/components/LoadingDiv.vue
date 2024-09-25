<script setup lang="ts">
const { apiresponse, selected_topic } = defineProps<{
  apiresponse: object | null
  selected_topic: string
}>()
</script>
<template>
  <div
    class="loader_box flex justify-center w-full mt-10"
    v-if="apiresponse == null && selected_topic != ''"
  >
    <div class="loading loading-bars loading-lg size-16"></div>
  </div>
</template>
<style scoped></style>

<!--  -->
