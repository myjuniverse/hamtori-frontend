<script setup>
import { ref } from 'vue'

const emit = defineEmits(['finish'])

// 질문 데이터 (10문항, 구체적 상황)
const questions = [
  {
    id: 1,
    text: "단톡방이 갑자기 조용해졌다. 나는?",
    options: [
      { text: "\"왜 갑자기 조용해ㅋㅋ\" 하고 먼저 말 건다", groups: ['group1', 'group3'] },
      { text: "눈치 보면서 누가 말할 때까지 기다린다", groups: ['group4', 'group5'] }
    ]
  },
  {
    id: 2,
    text: "친구가 맛집을 찾아달라고 한다. 나는?",
    options: [
      { text: "일단 아무 데나 가서 먹어보자고 한다", groups: ['group2', 'group3'] },
      { text: "리뷰 보고 별점 확인하고 골라준다", groups: ['group4'] }
    ]
  },
  {
    id: 3,
    text: "편의점에서 1+1 과자를 발견했다!",
    options: [
      { text: "일단 집어든다. 고민은 계산대 가서", groups: ['group2'] },
      { text: "진짜 먹을 건지 생각하고 결정한다", groups: ['group1', 'group4'] }
    ]
  },
  {
    id: 4,
    text: "관심 있는 사람이 내 인스타 스토리를 봤다!",
    options: [
      { text: "바로 DM 보내거나 반응한다", groups: ['group2', 'group3'] },
      { text: "괜히 의식되지만 아무렇지 않은 척한다", groups: ['group4', 'group5'] }
    ]
  },
  {
    id: 5,
    text: "MT 총무를 맡아달라는 연락이 왔다.",
    options: [
      { text: "귀찮지만 내가 하면 잘할 것 같아서 한다", groups: ['group3'] },
      { text: "누가 하겠지 하고 조용히 빠진다", groups: ['group5'] }
    ]
  },
  {
    id: 6,
    text: "갑자기 오늘 저녁에 번개 모임이 잡혔다!",
    options: [
      { text: "오 좋아! 바로 나갈 준비한다", groups: ['group2', 'group3'] },
      { text: "음... 일단 집에서 쉬고 싶은데", groups: ['group5'] }
    ]
  },
  {
    id: 7,
    text: "친구가 고민 상담을 요청했다.",
    options: [
      { text: "일단 만나서 얘기 들어주고 같이 해결한다", groups: ['group1'] },
      { text: "카톡으로 천천히 들으면서 조언해준다", groups: ['group4', 'group5'] }
    ]
  },
  {
    id: 8,
    text: "여행 계획을 짤 때 나는?",
    options: [
      { text: "대충 숙소만 잡고 가서 정한다", groups: ['group2', 'group5'] },
      { text: "동선, 맛집, 카페까지 다 짜놓는다", groups: ['group4'] }
    ]
  },
  {
    id: 9,
    text: "친구들이 내 의견을 무시하는 것 같다.",
    options: [
      { text: "\"야 내 말 좀 들어봐\" 하고 다시 말한다", groups: ['group3'] },
      { text: "그냥 흐름 따라가고 나중에 얘기한다", groups: ['group1', 'group5'] }
    ]
  },
  {
    id: 10,
    text: "주말 아침, 알람 없이 일어났다. 나는?",
    options: [
      { text: "뭐라도 해야 할 것 같아서 일단 일어난다", groups: ['group3', 'group4'] },
      { text: "행복하게 다시 눈 감고 더 잔다", groups: ['group2', 'group5'] }
    ]
  }
]

const currentIndex = ref(0)
const scores = ref({
  group1: 0,
  group2: 0,
  group3: 0,
  group4: 0,
  group5: 0
})

// 각 질문별 선택 기록 저장 (인덱스로 접근)
const selectedAnswers = ref({})

const currentQuestion = () => questions[currentIndex.value]
const progress = () => ((currentIndex.value) / questions.length) * 100

// 현재 질문에서 선택된 옵션 인덱스
const getSelectedIndex = () => selectedAnswers.value[currentIndex.value] ?? -1

const selectOption = (option, optionIndex) => {
  // 이전에 선택한 적 있으면 점수 빼기
  const prevIndex = selectedAnswers.value[currentIndex.value]
  if (prevIndex !== undefined) {
    const prevOption = currentQuestion().options[prevIndex]
    prevOption.groups.forEach(group => {
      scores.value[group]--
    })
  }

  // 현재 선택 저장
  selectedAnswers.value[currentIndex.value] = optionIndex

  // 점수 추가
  option.groups.forEach(group => {
    scores.value[group]++
  })

  // 다음 질문 또는 결과 (약간의 딜레이)
  setTimeout(() => {
    if (currentIndex.value < questions.length - 1) {
      currentIndex.value++
    } else {
      emit('finish', scores.value)
    }
  }, 300)
}

// 이전 질문으로 돌아가기
const goBack = () => {
  if (currentIndex.value > 0) {
    currentIndex.value--
  }
}
</script>

<template>
  <div class="question-page">
    <!-- 메인 콘텐츠 -->
    <div class="main-content">
      <!-- 질문 영역 -->
      <div class="question-area">
        <h2 class="question-text">{{ currentQuestion().text }}</h2>
      </div>

      <!-- 선택지 (질문 바로 아래) -->
      <div class="options-area">
        <button
          v-for="(option, index) in currentQuestion().options"
          :key="`q${currentIndex}-opt${index}`"
          class="option-button"
          :class="{ selected: getSelectedIndex() === index }"
          @click="selectOption(option, index)"
        >
          {{ option.text }}
        </button>
      </div>

      <!-- 뒤로가기 버튼 영역 (항상 공간 차지, 첫 질문에서는 투명) -->
      <div class="back-button-area">
        <button
          class="back-button"
          :class="{ invisible: currentIndex === 0 }"
          @click="goBack"
          :disabled="currentIndex === 0"
        >
          <span class="back-arrow">&lt;</span>
        </button>
      </div>
    </div>

    <!-- 하단 진행 표시 -->
    <div class="bottom-progress">
      <div class="progress-bubble">
        <span class="progress-number">{{ currentIndex + 1 }}/{{ questions.length }}</span>
        <div class="bubble-tail"></div>
      </div>
      <div class="hamster-walk">
        <span class="hamster-icon" :style="{ left: progress() + '%' }">🐹</span>
        <div class="walk-track">
          <div class="walk-fill" :style="{ width: progress() + '%' }"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.question-page {
  min-height: calc(100vh - 52px);
  display: flex;
  flex-direction: column;
  padding: 20px 20px 140px;
  background: linear-gradient(180deg, #FFF9F0 0%, #FFE8D0 100%);
  position: relative;
}

/* 메인 콘텐츠 */
.main-content {
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
}

/* 질문 영역 */
.question-area {
  margin-bottom: 32px;
  padding: 0 20px;
}

.question-text {
  font-size: 22px;
  font-weight: 700;
  color: #4A4A4A;
  text-align: center;
  line-height: 1.5;
}

/* 선택지 */
.options-area {
  display: flex;
  flex-direction: column;
  gap: 16px;
  padding: 0 20px;
}

.option-button {
  width: 100%;
  padding: 20px 24px;
  font-size: 16px;
  font-weight: 600;
  color: #4A4A4A;
  background: white;
  border: 2px solid #FFD4B8;
  border-radius: 16px;
  cursor: pointer;
  transition: all 0.2s ease;
  text-align: left;
  line-height: 1.4;
}

.option-button:hover {
  border-color: #FF8C42;
  background: #FFF5EE;
}

.option-button.selected {
  border-color: #FF8C42;
  background: #FF8C42;
  color: white;
}

/* 뒤로가기 버튼 영역 */
.back-button-area {
  margin-top: 20px;
  padding-left: 20px;
  height: 44px;
}

.back-button {
  width: 44px;
  height: 44px;
  border-radius: 50%;
  background: #FF8C42;
  border: none;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease;
  box-shadow: 0 2px 8px rgba(255, 140, 66, 0.3);
}

.back-button:hover:not(.invisible) {
  transform: scale(1.05);
  box-shadow: 0 4px 12px rgba(255, 140, 66, 0.4);
}

.back-button.invisible {
  opacity: 0;
  pointer-events: none;
}

.back-arrow {
  color: white;
  font-size: 18px;
  font-weight: bold;
}

/* 하단 진행 표시 */
.bottom-progress {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background: white;
  padding: 20px 40px 28px;
  border-top: 1px solid #FFE8D0;
  box-shadow: 0 -4px 20px rgba(0, 0, 0, 0.05);
}

/* 말풍선 스타일 진행 표시 */
.progress-bubble {
  position: relative;
  display: inline-block;
  background: white;
  border: 2px solid #4A4A4A;
  border-radius: 20px;
  padding: 8px 16px;
  margin-bottom: 16px;
}

.progress-number {
  font-size: 14px;
  font-weight: 700;
  color: #4A4A4A;
}

.bubble-tail {
  position: absolute;
  bottom: -8px;
  left: 20px;
  width: 0;
  height: 0;
  border-left: 6px solid transparent;
  border-right: 6px solid transparent;
  border-top: 8px solid #4A4A4A;
}

.bubble-tail::after {
  content: '';
  position: absolute;
  top: -10px;
  left: -5px;
  width: 0;
  height: 0;
  border-left: 5px solid transparent;
  border-right: 5px solid transparent;
  border-top: 7px solid white;
}

.hamster-walk {
  position: relative;
  width: 100%;
  padding-top: 35px;
}

.hamster-icon {
  position: absolute;
  top: 0;
  font-size: 28px;
  transform: translateX(-50%);
  transition: left 0.3s ease;
  animation: hamsterWalk 0.4s steps(2) infinite;
}

@keyframes hamsterWalk {
  0%, 100% { transform: translateX(-50%) translateY(0) scaleX(1); }
  50% { transform: translateX(-50%) translateY(-3px) scaleX(1); }
}

.walk-track {
  width: 100%;
  height: 8px;
  background: #EEE;
  border-radius: 4px;
  overflow: hidden;
}

.walk-fill {
  height: 100%;
  background: linear-gradient(90deg, #4CAF50, #8BC34A);
  border-radius: 4px;
  transition: width 0.3s ease;
}

/* 모바일 대응 */
@media (max-width: 480px) {
  .question-text {
    font-size: 18px;
  }

  .option-button {
    padding: 16px 20px;
    font-size: 15px;
  }

  .back-button {
    width: 40px;
    height: 40px;
  }

  .hamster-icon {
    font-size: 24px;
  }

  .bottom-progress {
    padding: 20px 20px 28px;
  }
}
</style>
