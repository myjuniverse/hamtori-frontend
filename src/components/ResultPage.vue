<script setup>
import { computed } from 'vue'

const props = defineProps({
  character: String,
  scores: Object
})

const emit = defineEmits(['restart'])

// 캐릭터 결과 데이터
const characterData = {
  group1: {
    name: '햄토리',
    emoji: '🐹',
    title: '순수하지만 중심이 되는 햄토리형',
    description: '밝고 활발하며 누구와도 잘 어울리는 타입입니다. 순수한 태도로 관계를 이어가는 중심 역할을 합니다.',
    traits: ['자연스럽게 사람들과 어울림', '주변에서 호감을 쉽게 얻음', '우정과 관계를 중요하게 생각'],
    color: '#FFB347'
  },
  group2: {
    name: '얌냠이',
    emoji: '🍙',
    title: '먹는 게 최우선인 얌냠이형',
    description: '먹는 것에 대한 집착이 강하고, 안정감을 중요하게 생각하는 타입입니다.',
    traits: ['기본적인 욕구에 충실', '감정 표현이 직접적', '단순하지만 인간적인 매력'],
    color: '#98D8AA'
  },
  group3: {
    name: '대장',
    emoji: '👑',
    title: '강하지만 감정 기복 있는 대장형',
    description: '리더 역할을 하지만, 감정과 자존심이 강한 타입입니다. 중심에 서면서도 종종 트러블을 만드는 성향입니다.',
    traits: ['힘과 존재감으로 중심에 섬', '책임감과 리더십 보유', '허세와 자신감이 섞여 있음'],
    color: '#FF6B6B'
  },
  group4: {
    name: '범생이',
    emoji: '📚',
    title: '지식 기반 해결형 범생이형',
    description: '이성적이고 성실하게 문제를 해결하는 타입입니다. 기본적으로 안정적인 판단을 합니다.',
    traits: ['지식과 논리 기반 행동', '차분하지만 때론 감정적', '균형 잡힌 이성형 캐릭터'],
    color: '#4ECDC4'
  },
  group5: {
    name: '쿨쿨이',
    emoji: '💤',
    title: '거의 항상 자는 쿨쿨이형',
    description: '활동보다 휴식을 우선하는 타입입니다. 겉으로는 비활동적이지만, 필요할 때 핵심을 짚습니다.',
    traits: ['대부분의 시간을 쉬는 데 사용', '결정적 순간에 정확한 판단', '저에너지 관찰형 캐릭터'],
    color: '#A594F9'
  }
}

const result = computed(() => characterData[props.character] || characterData.group1)

const handleRestart = () => {
  emit('restart')
}

const handleShare = () => {
  if (navigator.share) {
    navigator.share({
      title: '햄토리 테스트 결과',
      text: `나의 햄토리 유형은 "${result.value.name}"! 너도 테스트 해봐!`,
      url: window.location.href
    })
  } else {
    // 클립보드 복사
    navigator.clipboard.writeText(window.location.href)
    alert('링크가 복사되었습니다!')
  }
}
</script>

<template>
  <div class="result-page">
    <div class="content">
      <!-- 결과 카드 -->
      <div class="result-card" :style="{ borderColor: result.color }">
        <div class="character-emoji">{{ result.emoji }}</div>
        <div class="character-name" :style="{ color: result.color }">{{ result.name }}</div>
        <h2 class="character-title">{{ result.title }}</h2>
        <p class="character-description">{{ result.description }}</p>

        <!-- 특징 리스트 -->
        <ul class="traits-list">
          <li v-for="(trait, index) in result.traits" :key="index">
            {{ trait }}
          </li>
        </ul>
      </div>

      <!-- 버튼 영역 -->
      <div class="button-area">
        <button class="share-button" @click="handleShare">
          결과 공유하기
        </button>
        <button class="restart-button" @click="handleRestart">
          다시 테스트하기
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.result-page {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 20px;
  background: linear-gradient(180deg, #FFF9F0 0%, #FFE8D0 100%);
}

.content {
  max-width: 500px;
  width: 100%;
  padding: 20px;
}

/* 결과 카드 */
.result-card {
  background: white;
  border-radius: 24px;
  padding: 40px 24px;
  text-align: center;
  border: 3px solid;
  box-shadow: 0 8px 30px rgba(0, 0, 0, 0.08);
  margin-bottom: 24px;
}

.character-emoji {
  font-size: 80px;
  margin-bottom: 16px;
}

.character-name {
  font-size: 28px;
  font-weight: 800;
  margin-bottom: 8px;
}

.character-title {
  font-size: 18px;
  font-weight: 700;
  color: #4A4A4A;
  margin-bottom: 16px;
}

.character-description {
  font-size: 15px;
  color: #666;
  line-height: 1.6;
  margin-bottom: 24px;
}

.traits-list {
  list-style: none;
  padding: 0;
  text-align: left;
  background: #F9F9F9;
  border-radius: 12px;
  padding: 16px 20px;
}

.traits-list li {
  font-size: 14px;
  color: #555;
  padding: 8px 0;
  border-bottom: 1px solid #EEE;
}

.traits-list li:last-child {
  border-bottom: none;
}

.traits-list li::before {
  content: '✓ ';
  color: #FF8C42;
  font-weight: bold;
}

/* 버튼 영역 */
.button-area {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.share-button {
  width: 100%;
  padding: 18px;
  font-size: 16px;
  font-weight: 700;
  color: white;
  background: linear-gradient(135deg, #FF8C42 0%, #FF6B35 100%);
  border: none;
  border-radius: 50px;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(255, 140, 66, 0.4);
}

.share-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(255, 140, 66, 0.5);
}

.restart-button {
  width: 100%;
  padding: 18px;
  font-size: 16px;
  font-weight: 700;
  color: #FF8C42;
  background: white;
  border: 2px solid #FF8C42;
  border-radius: 50px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.restart-button:hover {
  background: #FFF5EE;
}

/* 모바일 대응 */
@media (max-width: 480px) {
  .character-emoji {
    font-size: 60px;
  }

  .character-name {
    font-size: 24px;
  }

  .result-card {
    padding: 30px 20px;
  }
}
</style>
