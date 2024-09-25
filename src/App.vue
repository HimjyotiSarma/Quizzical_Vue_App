<script setup lang="ts">
import { ref, watchEffect } from 'vue'
import HeaderArea from './components/HeaderArea.vue'
import SelectQuizTopic from './components/SelectQuizTopic.vue'
import LoadingDiv from './components/LoadingDiv.vue'
import QuestionComponent from './components/QuestionComponent.vue'
import ScoreCard from './components/ScoreCard.vue'

const quizStart = ref(true)
const quizStatus = ref('Select your Topic and Start the Quiz')
const quizScore = ref(0)
const isGameFinished = ref(false)
const title = ref('Quiz App in Vue!')
const selected_topic = ref('')
let apiresponse = ref<any>(null)
let isResponseFetched = ref(false)

watchEffect(async () => {
  isResponseFetched.value = false
  const url = new URL('https://quizapi.io/api/v1/questions')
  url.searchParams.append('apiKey', import.meta.env.VITE_API_TOKEN)
  url.searchParams.append('limit', '10')

  if (selected_topic.value != '') {
    url.searchParams.append('category', selected_topic.value)

    const response = await fetch(url, {
      method: 'GET',
      headers: {
        'Content-Type': 'application/json'
      }
    })

    let data = await response.json()
    apiresponse.value = data
    if (data) {
      quizStatus.value = 'Select your Answer and Click Next/Finish'
    }
    isResponseFetched.value = true
  }
})
</script>

<template>
  <HeaderArea
    :title="title"
    :quizStatus="quizStatus"
    :quizStart="quizStart"
    v-if="!isGameFinished"
  />

  <SelectQuizTopic
    v-if="apiresponse == null"
    @topic_selected="(topic) => (selected_topic = topic)"
  />
  <LoadingDiv :apiresponse="apiresponse" :selected_topic="selected_topic" />
  <QuestionComponent
    v-if="apiresponse != null && !isGameFinished"
    :apiresponse="apiresponse"
    @game_finished_status="isGameFinished = true"
    @user_score="(score) => (quizScore = score)"
  />
  <ScoreCard
    v-if="isGameFinished"
    :quizScore="quizScore"
    :totalScore="apiresponse != null ? apiresponse.length : 0"
  />
</template>

<style></style>
