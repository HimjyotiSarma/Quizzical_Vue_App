<script setup lang="ts">
import { ref } from 'vue'
defineEmits(['topic_selected'])
const QuestionTopics = ref(['Linux', 'bash', 'Docker', 'SQL', 'CMS', 'Code', 'Devops'])
</script>
<template>
  <div class="grid grid-cols-2 md:grid-cols-3 gap-4 container py-8 px-8" id="topic_list">
    <div
      class="h-24 justify-center items-center text-3xl btn btn-neutral"
      v-for="(topic, index) in QuestionTopics"
      :key="index"
      @click="$emit('topic_selected', topic)"
    >
      {{ topic }}
    </div>
  </div>
</template>
<script scoped></script>
