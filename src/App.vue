<script setup>
import { ref } from 'vue'
import StartPage from './components/StartPage.vue'
import QuestionPage from './components/QuestionPage.vue'
import ResultPage from './components/ResultPage.vue'

// 페이지 상태: 'start' | 'question' | 'result'
const currentPage = ref('start')

// 점수 저장 (5개 그룹)
const scores = ref({
  group1: 0, // 중심/관계형 (햄토리, 머플러, 리본)
  group2: 0, // 본능/감정형 (얌냠이, 미니미니, 랑돌이)
  group3: 0, // 리더/행동형 (대장, 랑순이, 아따아따)
  group4: 0, // 이성/판단형 (안경, 범생이, 팬더)
  group5: 0  // 회피/느긋형 (모자, 쿨쿨이, 딩가딩)
})

// 결과 캐릭터
const resultCharacter = ref(null)

const startQuiz = () => {
  currentPage.value = 'question'
}

const finishQuiz = (finalScores) => {
  scores.value = finalScores
  // 가장 높은 점수 그룹 찾기
  const maxGroup = Object.entries(finalScores).reduce((a, b) =>
    a[1] > b[1] ? a : b
  )[0]
  resultCharacter.value = maxGroup
  currentPage.value = 'result'
}

const restartQuiz = () => {
  scores.value = { group1: 0, group2: 0, group3: 0, group4: 0, group5: 0 }
  resultCharacter.value = null
  currentPage.value = 'start'
}
</script>

<template>
  <div class="app">
    <StartPage
      v-if="currentPage === 'start'"
      @start="startQuiz"
    />
    <QuestionPage
      v-else-if="currentPage === 'question'"
      @finish="finishQuiz"
    />
    <ResultPage
      v-else-if="currentPage === 'result'"
      :character="resultCharacter"
      :scores="scores"
      @restart="restartQuiz"
    />
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

body {
  font-family: 'Pretendard', -apple-system, BlinkMacSystemFont, sans-serif;
  background-color: #FFF9F0;
  min-height: 100vh;
}

.app {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}
</style>
