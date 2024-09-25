<script lang="ts" setup>
import { computed, ref } from 'vue'
import SingleQA from './SingleQA.vue'

let currentQuestion = ref(0)
let GameFinishedStatus = ref(false)
const totalAttemptedQuestion = ref(0)

const emit = defineEmits(['game_finished_status', 'user_score'])
const quizScore = ref<number>(0)

const { apiresponse } = defineProps<{
  apiresponse: Array<any>
}>()

const computedQuestionsLength = computed(() => {
  return apiresponse ? apiresponse.length : 0
})
</script>
<template>
  <div class="container">
    <SingleQA
      v-for="(questionDetail, index) in apiresponse"
      :key="index"
      :class="{
        active_question: currentQuestion == index,
        inactive_question: currentQuestion !== index
      }"
      :currentQuestion="currentQuestion"
      :indexId="index"
      :questionDetail="questionDetail"
      :totalQuestion="computedQuestionsLength"
      :attemptedQuestion="totalAttemptedQuestion"
      @update_question="(currentQuestionId) => (currentQuestion = currentQuestionId)"
      @finish_game="
        () => {
          GameFinishedStatus = true
          emit('game_finished_status')
          emit('user_score', quizScore)
        }
      "
      @update_score="quizScore += 1"
      @attempted_question="totalAttemptedQuestion += 1"
      :quizScore="quizScore"
    />
  </div>
</template>

<style lang="css" scoped>
.active_question {
  display: flex;
}
.inactive_question {
  display: none;
}
</style>
