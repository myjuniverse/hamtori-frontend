<script setup>
import { ref } from 'vue'
import NavBar from './components/NavBar.vue'
import StartPage from './components/StartPage.vue'
import QuestionPage from './components/QuestionPage.vue'
import ResultPage from './components/ResultPage.vue'
import CharactersPage from './components/CharactersPage.vue'

// 페이지 상태: 'home' | 'test' | 'characters' | 'result'
const currentPage = ref('home')

// 점수 저장 (5개 그룹)
const scores = ref({
  group1: 0,
  group2: 0,
  group3: 0,
  group4: 0,
  group5: 0
})

// 결과 캐릭터 (그룹 + 세분화 인덱스)
const resultCharacter = ref(null)
const resultSubIndex = ref(0)

// 퀴즈 리셋 키 (테스트하기 탭 클릭 시 새로 시작)
const quizKey = ref(0)

const navigateTo = (page) => {
  if (page === 'test') {
    // 테스트하기 탭 클릭 시 퀴즈 초기화
    scores.value = { group1: 0, group2: 0, group3: 0, group4: 0, group5: 0 }
    quizKey.value++
  }
  currentPage.value = page
}

const startQuiz = () => {
  scores.value = { group1: 0, group2: 0, group3: 0, group4: 0, group5: 0 }
  quizKey.value++
  currentPage.value = 'test'
}

const finishQuiz = (result) => {
  const { scores: finalScores, subScores } = result

  // 1. 가장 높은 그룹 찾기
  scores.value = finalScores
  const maxGroup = Object.entries(finalScores).reduce((a, b) =>
    a[1] > b[1] ? a : b
  )[0]
  resultCharacter.value = maxGroup

  // 2. 세분화 점수로 캐릭터 인덱스 결정 (0, 1, 2)
  const { sub1, sub2, sub3 } = subScores
  if (sub1 >= sub2 && sub1 >= sub3) {
    resultSubIndex.value = 0  // 첫번째 캐릭터
  } else if (sub2 >= sub1 && sub2 >= sub3) {
    resultSubIndex.value = 1  // 두번째 캐릭터
  } else {
    resultSubIndex.value = 2  // 세번째 캐릭터
  }

  currentPage.value = 'result'
}

const restartQuiz = () => {
  scores.value = { group1: 0, group2: 0, group3: 0, group4: 0, group5: 0 }
  quizKey.value++
  currentPage.value = 'test'
}

// 네비게이션 표시 여부 (결과 페이지에서는 숨김)
const showNav = () => currentPage.value !== 'result'

// 현재 활성 탭 (result일 때는 test로 표시)
const activeTab = () => currentPage.value === 'result' ? 'test' : currentPage.value
</script>

<template>
  <div class="app">
    <NavBar
      v-if="showNav()"
      :currentPage="activeTab()"
      @navigate="navigateTo"
    />
    <StartPage
      v-if="currentPage === 'home'"
      @start="startQuiz"
    />
    <QuestionPage
      v-else-if="currentPage === 'test'"
      :key="quizKey"
      @finish="finishQuiz"
    />
    <CharactersPage
      v-else-if="currentPage === 'characters'"
    />
    <ResultPage
      v-else-if="currentPage === 'result'"
      :character="resultCharacter"
      :subIndex="resultSubIndex"
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
