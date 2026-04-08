<script setup>
import { ref } from 'vue'

const selectedCharacter = ref(null)

const characters = [
  // 중심/관계형
  {
    id: 'hamtori',
    name: '햄토리',
    image: '/characters/hamtori.png',
    emoji: null,
    title: '순수하지만 중심이 되는 햄토리형',
    description: '밝고 활발하며 누구와도 잘 어울리는 타입입니다.',
    traits: ['자연스럽게 사람들과 어울림', '주변에서 호감을 쉽게 얻음', '우정과 관계를 중요하게 생각'],
    color: '#FFB347',
    group: '중심/관계형'
  },
  {
    id: 'ribbon',
    name: '리본',
    image: null,
    emoji: '🎀',
    title: '섬세하고 감정이 깊은 리본형',
    description: '감정 표현이 풍부하고, 관계에 민감하게 반응하는 타입입니다.',
    traits: ['감정선이 뚜렷함', '특정 대상에게 마음 집중', '내면에서 질투나 상처도 크게 느낌'],
    color: '#FF9ECD',
    group: '중심/관계형'
  },
  {
    id: 'muffler',
    name: '머플러',
    image: null,
    emoji: '🧣',
    title: '차분하게 챙기는 머플러형',
    description: '다른 사람을 이해하고 도와주는 역할을 맡는 타입입니다.',
    traits: ['주변 사람을 챙김', '약한 존재를 보호', '조용히 중심을 잡아줌'],
    color: '#87CEEB',
    group: '중심/관계형'
  },
  // 본능/감정형
  {
    id: 'yamnyami',
    name: '얌냠이',
    image: null,
    emoji: '🍙',
    title: '먹는 게 최우선인 얌냠이형',
    description: '먹는 것에 대한 집착이 강하고 안정감을 중요시합니다.',
    traits: ['기본적인 욕구에 충실', '감정 표현이 직접적', '단순하지만 인간적인 매력'],
    color: '#98D8AA',
    group: '본능/감정형'
  },
  {
    id: 'minimini',
    name: '미니미니',
    image: null,
    emoji: '👶',
    title: '통제 안 되는 막내 미니미니형',
    description: '생각보다 행동이 먼저 나오는 타입입니다.',
    traits: ['호기심과 충동으로 움직임', '타인을 생각하는 마음이 큼', '미숙하지만 순수한 매력'],
    color: '#FFD93D',
    group: '본능/감정형'
  },
  {
    id: 'rangdori',
    name: '랑돌이',
    image: null,
    emoji: '💕',
    title: '자유로운 카사노바 랑돌이형',
    description: '관계에서 가볍고 적극적인 태도를 보이는 타입입니다.',
    traits: ['관계를 가볍게 접근', '자유롭고 즉흥적', '매력적이지만 안정감 부족'],
    color: '#FF6B6B',
    group: '본능/감정형'
  },
  // 리더/행동형
  {
    id: 'daejang',
    name: '대장',
    image: null,
    emoji: '👑',
    title: '강하지만 감정 기복 있는 대장형',
    description: '리더 역할을 하지만 감정과 자존심이 강합니다.',
    traits: ['힘과 존재감으로 중심에 섬', '책임감과 리더십 보유', '허세와 자신감이 섞여 있음'],
    color: '#FF6B6B',
    group: '리더/행동형'
  },
  {
    id: 'rangsuni',
    name: '랑순이',
    image: null,
    emoji: '⚡',
    title: '직진형 행동파 랑순이형',
    description: '에너지가 넘치고, 생각보다 행동이 빠른 타입입니다.',
    traits: ['활동성과 추진력이 강함', '먼저 다가가는 편', '감정 표현이 솔직함'],
    color: '#FF8C42',
    group: '리더/행동형'
  },
  {
    id: 'attaatta',
    name: '아따아따',
    image: null,
    emoji: '🎭',
    title: '분위기형 개그 아따아따형',
    description: '사람들과 쉽게 어울리지만, 말과 행동이 과한 타입입니다.',
    traits: ['친화력이 매우 좋음', '평소엔 가볍지만 의외로 진지함', '가벼움과 진중함 공존'],
    color: '#9B59B6',
    group: '리더/행동형'
  },
  // 이성/판단형
  {
    id: 'glasses',
    name: '안경',
    image: null,
    emoji: '🤓',
    title: '계산적이고 잘난척하는 안경형',
    description: '이미지와 평가를 중요하게 생각하는 타입입니다.',
    traits: ['판단과 계획 우선', '비교하거나 경쟁하는 성향', '이성적이지만 관계에서 복잡해짐'],
    color: '#5DADE2',
    group: '이성/판단형'
  },
  {
    id: 'nerd',
    name: '범생이',
    image: null,
    emoji: '📚',
    title: '지식 기반 해결형 범생이형',
    description: '이성적이고 성실하게 문제를 해결합니다.',
    traits: ['지식과 논리 기반 행동', '차분하지만 때론 감정적', '균형 잡힌 이성형'],
    color: '#4ECDC4',
    group: '이성/판단형'
  },
  {
    id: 'panda',
    name: '팬더',
    image: null,
    emoji: '🐼',
    title: '조용히 만드는 제작형 팬더형',
    description: '말보다 행동으로 결과를 만드는 타입입니다.',
    traits: ['꾸준히 만들어냄', '창의적 작업에 강점', '실용 중심형'],
    color: '#2C3E50',
    group: '이성/판단형'
  },
  // 회피/느긋형
  {
    id: 'hat',
    name: '모자',
    image: null,
    emoji: '🧢',
    title: '수줍고 숨는 모자형',
    description: '낯을 많이 가리고, 스스로를 숨기려는 경향이 있습니다.',
    traits: ['새로운 상황에서 드러나지 않음', '특정 물건에 강한 애착', '내향적이고 방어적'],
    color: '#A594F9',
    group: '회피/느긋형'
  },
  {
    id: 'sleepy',
    name: '쿨쿨이',
    image: null,
    emoji: '💤',
    title: '거의 항상 자는 쿨쿨이형',
    description: '활동보다 휴식을 우선하지만 필요할 때 핵심을 짚습니다.',
    traits: ['대부분의 시간을 쉬는 데 사용', '결정적 순간에 정확한 판단', '저에너지 관찰형'],
    color: '#85C1E9',
    group: '회피/느긋형'
  },
  {
    id: 'dingading',
    name: '딩가딩',
    image: null,
    emoji: '✨',
    title: '떠돌며 조언하는 딩가딩형',
    description: '한곳에 머무르지 않고 자유롭게 움직이는 타입입니다.',
    traits: ['일정한 틀에 묶이지 않음', '필요할 때만 등장', '거리 두는 조언자형'],
    color: '#F39C12',
    group: '회피/느긋형'
  }
]

const selectCharacter = (char) => {
  selectedCharacter.value = char
}

const closeModal = () => {
  selectedCharacter.value = null
}
</script>

<template>
  <div class="characters-page">
    <div class="content">
      <h1 class="page-title">햄토리 친구들</h1>
      <p class="page-subtitle">테스트로 만날 수 있는 캐릭터들이에요!</p>

      <div class="character-grid">
        <div
          v-for="char in characters"
          :key="char.id"
          class="character-item"
          @click="selectCharacter(char)"
        >
          <div class="char-image-wrapper" :style="{ borderColor: char.color }">
            <img v-if="char.image" :src="char.image" :alt="char.name" class="char-image" />
            <span v-else class="char-emoji">{{ char.emoji }}</span>
          </div>
          <span class="char-name">{{ char.name }}</span>
        </div>
      </div>
    </div>

    <!-- 상세 모달 -->
    <div v-if="selectedCharacter" class="modal-overlay" @click="closeModal">
      <div class="modal-content" :style="{ borderColor: selectedCharacter.color }" @click.stop>
        <button class="modal-close" @click="closeModal">×</button>

        <div class="modal-header">
          <img v-if="selectedCharacter.image" :src="selectedCharacter.image" :alt="selectedCharacter.name" class="modal-image" />
          <span v-else class="modal-emoji">{{ selectedCharacter.emoji }}</span>
          <div class="modal-info">
            <span class="modal-group" :style="{ background: selectedCharacter.color }">{{ selectedCharacter.group }}</span>
            <h2 class="modal-name" :style="{ color: selectedCharacter.color }">{{ selectedCharacter.name }}</h2>
            <p class="modal-title">{{ selectedCharacter.title }}</p>
          </div>
        </div>

        <p class="modal-description">{{ selectedCharacter.description }}</p>

        <ul class="modal-traits">
          <li v-for="(trait, idx) in selectedCharacter.traits" :key="idx">{{ trait }}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<style scoped>
.characters-page {
  min-height: calc(100vh - 52px);
  padding: 20px;
  background: linear-gradient(180deg, #FFF9F0 0%, #FFE8D0 100%);
}

.content {
  max-width: 600px;
  margin: 0 auto;
}

.page-title {
  font-size: 28px;
  font-weight: 800;
  color: #FF8C42;
  text-align: center;
  margin-bottom: 8px;
}

.page-subtitle {
  font-size: 14px;
  color: #999;
  text-align: center;
  margin-bottom: 32px;
}

/* 캐릭터 그리드 */
.character-grid {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 16px;
}

.character-item {
  display: flex;
  flex-direction: column;
  align-items: center;
  cursor: pointer;
  transition: transform 0.2s ease;
}

.character-item:hover {
  transform: scale(1.05);
}

.char-image-wrapper {
  width: 80px;
  height: 80px;
  background: white;
  border-radius: 50%;
  border: 3px solid;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
  overflow: hidden;
}

.char-image {
  width: 90%;
  height: 90%;
  object-fit: contain;
}

.char-emoji {
  font-size: 36px;
}

.char-name {
  font-size: 13px;
  font-weight: 600;
  color: #4A4A4A;
  text-align: center;
}

/* 모달 */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 20px;
}

.modal-content {
  background: white;
  border-radius: 24px;
  padding: 24px;
  max-width: 400px;
  width: 100%;
  border: 3px solid;
  position: relative;
  max-height: 80vh;
  overflow-y: auto;
}

.modal-close {
  position: absolute;
  top: 12px;
  right: 16px;
  background: none;
  border: none;
  font-size: 28px;
  color: #999;
  cursor: pointer;
}

.modal-close:hover {
  color: #333;
}

.modal-header {
  display: flex;
  align-items: center;
  gap: 16px;
  margin-bottom: 20px;
}

.modal-image {
  width: 80px;
  height: 80px;
  object-fit: contain;
}

.modal-emoji {
  font-size: 64px;
}

.modal-info {
  flex: 1;
}

.modal-group {
  display: inline-block;
  font-size: 11px;
  font-weight: 600;
  color: white;
  padding: 4px 10px;
  border-radius: 12px;
  margin-bottom: 8px;
}

.modal-name {
  font-size: 24px;
  font-weight: 800;
  margin-bottom: 4px;
}

.modal-title {
  font-size: 13px;
  color: #666;
}

.modal-description {
  font-size: 14px;
  color: #555;
  line-height: 1.6;
  margin-bottom: 16px;
}

.modal-traits {
  list-style: none;
  padding: 0;
  margin: 0;
  background: #F9F9F9;
  border-radius: 12px;
  padding: 12px 16px;
}

.modal-traits li {
  font-size: 13px;
  color: #666;
  padding: 6px 0;
  border-bottom: 1px solid #EEE;
}

.modal-traits li:last-child {
  border-bottom: none;
}

.modal-traits li::before {
  content: '• ';
  color: #FF8C42;
}

/* 모바일 대응 */
@media (max-width: 480px) {
  .character-grid {
    grid-template-columns: repeat(5, 1fr);
    gap: 12px;
  }

  .char-image-wrapper {
    width: 56px;
    height: 56px;
  }

  .char-emoji {
    font-size: 28px;
  }

  .char-name {
    font-size: 11px;
  }

  .page-title {
    font-size: 24px;
  }
}
</style>
