<script lang="ts" setup>
import { computed, ref } from 'vue'

let { currentQuestion, indexId, questionDetail, totalQuestion, attemptedQuestion } = defineProps<{
  currentQuestion: number
  totalQuestion: number
  indexId: number
  questionDetail: String | any
  attemptedQuestion: number
}>()

let selections = ref<Array<any>>([])
let hasAttempted = ref<boolean>(false)
let allOptions = [...Object.values(questionDetail['answers'])]
let correct_answers = ref<Array<any>>([])
let computeAnswerClass = (answer: string) => {
  if (selections.value.includes(answer) && correct_answers.value.includes(answer)) {
    return 'bg-green-900'
  } else if (selections.value.includes(answer) && !correct_answers.value.includes(answer)) {
    return 'bg-red-400'
  } else {
    return ''
  }
}

const emit = defineEmits(['update_question', 'finish_game', 'update_score', 'attempted_question'])

let handleQuestionPrevBtn = () => {
  if (hasAttempted.value == false) {
    alert('Attempt the Question First')
  }
  if (currentQuestion > 0 && hasAttempted.value) {
    emit('update_question', currentQuestion - 1)
  }
}
let handleQuestionNextBtn = () => {
  if (hasAttempted.value == false) {
    alert('Attempt the Question First')
  }
  if (hasAttempted.value) {
    emit('update_question', (currentQuestion + 1) % totalQuestion)
  }
}
let handleFinishQuiz = () => {
  emit('finish_game')
}

let checkAnswers = () => {
  Object.keys(questionDetail['correct_answers']).forEach((key, index) => {
    if (questionDetail['correct_answers'][key] == 'true') {
      correct_answers.value.push(allOptions[index])
    }
  })
}
checkAnswers()

let showAnswersPara = computed(() => {
  if (selections.value.length == correct_answers.value.length) {
    return 'visible'
  }
  return 'hidden'
})

console.log(allOptions)

let handleSelection = (answer: any) => {
  if (hasAttempted.value) return
  let isMultipleSelection = questionDetail['multiple_correct_answers'] === 'true'
  if (isMultipleSelection) {
    if (selections.value.includes(answer)) {
      let ansIndex = selections.value.indexOf(answer)
      selections.value.splice(ansIndex, 1)
    } else {
      selections.value.push(answer)
    }
  } else {
    selections.value = [answer]
  }
  if (selections.value.length == correct_answers.value.length) {
    let isAnswerCorrect = correct_answers.value.every((item) => selections.value.includes(item))
    if (isAnswerCorrect) {
      emit('update_score')
    }
    emit('attempted_question')
    hasAttempted.value = true
    return
  } // Stop further selections
}
</script>

<template>
  <!-- If the Question is Last then create a finish button and emit a game end boolean true -->
  <div class="container flex justify-center items-center flex-col">
    <div
      class="w-[75%] max-w-[800px] p-10"
      :id="'question-' + questionDetail['category'] + '-' + (indexId + 1)"
    >
      <div
        class="border-b-4 border-blue-900 w-full"
        v-if="questionDetail['multiple_correct_answers'] === 'true'"
      >
        {{ indexId + 1 }} of {{ totalQuestion }} Question ( Select
        {{ correct_answers.length }} option(s) )
      </div>
      <div class="border-b-4 border-blue-900 w-full" v-else>
        {{ indexId + 1 }} of {{ totalQuestion }} Question
      </div>
      <div class="text-3xl py-5">
        {{ questionDetail['question'] }}
      </div>
      <ul class="options flex flex-col items-center">
        <template v-for="(answer, index) in questionDetail['answers']" :key="index">
          <li
            v-if="answer !== null && answer !== ''"
            class="btn btn-accent w-full mb-4"
            :class="computeAnswerClass(answer)"
            @click="handleSelection(answer)"
          >
            {{ answer }}
          </li>
        </template>
      </ul>
      <p class="text-info" :style="{ visibility: showAnswersPara }">
        Correct Answers : {{ correct_answers.join(', ') }}
      </p>
    </div>
    <div class="navigate_btns container flex justify-center items-center mt-14">
      <button
        class="btn btn-info mr-8 w-20"
        :class="{ 'bg-slate-400': hasAttempted == false || currentQuestion == 0 }"
        @click="handleQuestionPrevBtn()"
      >
        Previous
      </button>

      <button
        class="btn btn-info ml-8 w-20"
        :class="{ 'bg-slate-400': hasAttempted == false }"
        @click="handleQuestionNextBtn()"
        v-if="attemptedQuestion < totalQuestion"
      >
        Next
      </button>
      <button class="btn btn-info ml-8 w-20" @click="handleFinishQuiz()" v-else>Finish</button>
    </div>
  </div>
</template>

<style lang="css" scoped></style>
