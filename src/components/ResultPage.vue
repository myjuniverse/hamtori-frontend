<script setup>
import { computed } from 'vue'

const props = defineProps({
  character: String,
  subIndex: {
    type: Number,
    default: 0
  },
  scores: Object
})

const emit = defineEmits(['restart'])

// 그룹별 캐릭터 데이터 (각 그룹 3명씩)
const characterGroups = {
  group1: [ // 중심/관계형
    {
      name: '햄토리',
      emoji: '🐹',
      title: '순수하지만 중심이 되는 햄토리형',
      description: '밝고 활발하며 누구와도 잘 어울리는 타입입니다. 순수한 태도로 관계를 이어가는 중심 역할입니다.',
      traits: ['자연스럽게 사람들과 어울림', '주변에서 호감을 쉽게 얻음', '우정과 관계를 중요하게 생각', '순수함과 정의감이 함께 있는 균형형'],
      color: '#FFB347'
    },
    {
      name: '리본',
      emoji: '🎀',
      title: '섬세하고 감정이 깊은 리본형',
      description: '감정 표현이 풍부하고, 관계에 민감하게 반응하는 타입입니다. 좋아하는 대상에 대한 애정이 깊습니다.',
      traits: ['감정선이 뚜렷하고 관계 중시', '특정 대상에게 마음이 집중됨', '겉으로는 차분하지만 내면은 질투나 상처도 큼', '감정 중심형 캐릭터'],
      color: '#FF9ECD'
    },
    {
      name: '머플러',
      emoji: '🧣',
      title: '차분하게 챙기는 머플러형',
      description: '다른 사람을 이해하고 도와주는 역할을 맡는 타입입니다. 특히 특정 대상에게 책임감 있게 행동합니다.',
      traits: ['주변 사람을 챙기고 중재하는 역할', '약한 존재를 자연스럽게 보호', '다정하지만 필요할 때는 단호함', '조용히 중심을 잡아주는 보조형'],
      color: '#87CEEB'
    }
  ],
  group2: [ // 본능/감정형
    {
      name: '얌냠이',
      emoji: '🍙',
      title: '먹는 게 최우선인 얌냠이형',
      description: '먹는 것에 대한 집착이 강하고, 안정감을 중요하게 생각하는 타입입니다.',
      traits: ['기본적인 욕구에 매우 충실', '익숙한 것을 유지하려는 경향', '감정 표현이 직접적', '단순하지만 인간적인 매력'],
      color: '#98D8AA'
    },
    {
      name: '미니미니',
      emoji: '👶',
      title: '통제 안 되는 막내 미니미니형',
      description: '생각보다 행동이 먼저 나오는 타입입니다. 제멋대로 움직이며 주변까지 휘말리게 만듭니다.',
      traits: ['감정과 행동이 직선적으로 연결', '호기심과 충동으로 움직임', '타인을 생각하는 마음이 큼', '미숙하지만 순수한 매력'],
      color: '#FFD93D'
    },
    {
      name: '랑돌이',
      emoji: '💕',
      title: '자유로운 카사노바 랑돌이형',
      description: '관계에서 가볍고 적극적인 태도를 보이는 타입입니다. 여러 사람에게 관심을 가지는 경향이 있습니다.',
      traits: ['관계를 가볍고 유연하게 접근', '여러 방향으로 관심 분산', '자유롭고 즉흥적인 행동', '매력적이지만 안정감 부족'],
      color: '#FF6B6B'
    }
  ],
  group3: [ // 리더/행동형
    {
      name: '대장',
      emoji: '👑',
      title: '강하지만 감정 기복 있는 대장형',
      description: '리더 역할을 하지만, 감정과 자존심이 강한 타입입니다. 중심에 서면서도 종종 트러블을 만듭니다.',
      traits: ['힘과 존재감으로 중심에 섬', '자존심과 감정 표현이 강함', '허세와 자신감이 섞여 있음', '책임감과 리더십은 분명함'],
      color: '#FF6B6B'
    },
    {
      name: '랑순이',
      emoji: '⚡',
      title: '직진형 행동파 랑순이형',
      description: '에너지가 넘치고, 생각보다 행동이 빠른 타입입니다. 감정 표현도 적극적입니다.',
      traits: ['활동성과 추진력이 강함', '원하는 것에 망설이지 않음', '먼저 다가가는 편', '활동 중심형 캐릭터'],
      color: '#FF8C42'
    },
    {
      name: '아따아따',
      emoji: '🎭',
      title: '분위기형 개그 아따아따형',
      description: '사람들과 쉽게 어울리지만, 말과 행동이 과한 타입입니다. 분위기를 만들지만 동시에 흐리기도 합니다.',
      traits: ['친화력이 매우 좋음', '과한 행동으로 반응이 엇갈림', '평소에는 가볍지만 의외로 진지함', '가벼움과 진중함이 공존'],
      color: '#9B59B6'
    }
  ],
  group4: [ // 이성/판단형
    {
      name: '안경',
      emoji: '🤓',
      title: '계산적이고 잘난척하는 안경형',
      description: '이미지와 평가를 중요하게 생각하는 타입입니다. 상황을 분석하고 전략적으로 움직입니다.',
      traits: ['감정보다 판단과 계획 우선', '어떻게 보일지 계속 고민', '비교하거나 경쟁하는 성향', '이성적이지만 관계에서 복잡해질 수 있음'],
      color: '#5DADE2'
    },
    {
      name: '범생이',
      emoji: '📚',
      title: '지식 기반 해결형 범생이형',
      description: '이성적이고 성실하게 문제를 해결하는 타입입니다. 기본적으로 안정적인 판단을 합니다.',
      traits: ['지식과 논리 기반 행동', '이론과 기준을 중요시', '차분하지만 때론 감정적', '균형 잡힌 이성형 캐릭터'],
      color: '#4ECDC4'
    },
    {
      name: '팬더',
      emoji: '🐼',
      title: '조용히 만드는 제작형 팬더형',
      description: '말보다 행동으로 결과를 만드는 타입입니다. 차분하지만 실질적인 능력이 있습니다.',
      traits: ['눈에 띄지 않지만 꾸준히 만들어냄', '창의적 작업에 강점', '감정 기복이 적고 안정적', '실용 중심형 캐릭터'],
      color: '#2C3E50'
    }
  ],
  group5: [ // 회피/느긋형
    {
      name: '모자',
      emoji: '🧢',
      title: '수줍고 숨는 모자형',
      description: '낯을 많이 가리고, 스스로를 숨기려는 경향이 있는 타입입니다. 익숙한 것에 대한 집착이 있습니다.',
      traits: ['새로운 상황에서 쉽게 드러나지 않음', '불편하면 스스로를 숨김', '특정 물건이나 습관에 강한 애착', '내향적이고 방어적인 캐릭터'],
      color: '#A594F9'
    },
    {
      name: '쿨쿨이',
      emoji: '💤',
      title: '거의 항상 자는 쿨쿨이형',
      description: '활동보다 휴식을 우선하는 타입입니다. 겉으로는 비활동적이지만, 필요할 때 핵심을 짚습니다.',
      traits: ['대부분의 시간을 쉬는 데 사용', '주변에 크게 개입하지 않음', '결정적 순간에 정확한 판단', '저에너지 관찰형 캐릭터'],
      color: '#85C1E9'
    },
    {
      name: '딩가딩',
      emoji: '✨',
      title: '떠돌며 조언하는 딩가딩형',
      description: '한곳에 머무르지 않고 자유롭게 움직이는 타입입니다. 직접 개입하기보다 방향만 제시합니다.',
      traits: ['일정한 틀에 묶이지 않음', '혼자 움직이며 필요할 때만 등장', '말은 모호하지만 결과적으로 맞음', '거리 두는 조언자형 캐릭터'],
      color: '#F39C12'
    }
  ]
}

// 그룹에서 세분화 인덱스에 해당하는 캐릭터 선택
const getCharacter = (group, subIndex) => {
  const characters = characterGroups[group]
  // subIndex가 범위를 벗어나면 0으로
  const index = subIndex >= 0 && subIndex < characters.length ? subIndex : 0
  return characters[index]
}

const result = computed(() => {
  if (props.character && characterGroups[props.character]) {
    return getCharacter(props.character, props.subIndex)
  }
  return characterGroups.group1[0]
})

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
