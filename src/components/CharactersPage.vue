<script setup>
const characters = [
  {
    name: '햄토리',
    emoji: '🐹',
    title: '순수하지만 중심이 되는 타입',
    description: '밝고 활발하며 누구와도 잘 어울립니다.',
    traits: ['자연스럽게 사람들과 어울림', '주변에서 호감을 쉽게 얻음', '우정과 관계를 중요하게 생각'],
    color: '#FFB347'
  },
  {
    name: '얌냠이',
    emoji: '🍙',
    title: '먹는 게 최우선인 타입',
    description: '먹는 것에 대한 집착이 강하고 안정감을 중요시합니다.',
    traits: ['기본적인 욕구에 충실', '감정 표현이 직접적', '단순하지만 인간적인 매력'],
    color: '#98D8AA'
  },
  {
    name: '대장',
    emoji: '👑',
    title: '강하지만 감정 기복 있는 리더 타입',
    description: '리더 역할을 하지만 감정과 자존심이 강합니다.',
    traits: ['힘과 존재감으로 중심에 섬', '책임감과 리더십 보유', '허세와 자신감이 섞여 있음'],
    color: '#FF6B6B'
  },
  {
    name: '범생이',
    emoji: '📚',
    title: '지식 기반 해결형 타입',
    description: '이성적이고 성실하게 문제를 해결합니다.',
    traits: ['지식과 논리 기반 행동', '차분하지만 때론 감정적', '균형 잡힌 이성형 캐릭터'],
    color: '#4ECDC4'
  },
  {
    name: '쿨쿨이',
    emoji: '💤',
    title: '거의 항상 자는 느긋한 타입',
    description: '활동보다 휴식을 우선하지만 필요할 때 핵심을 짚습니다.',
    traits: ['대부분의 시간을 쉬는 데 사용', '결정적 순간에 정확한 판단', '저에너지 관찰형 캐릭터'],
    color: '#A594F9'
  }
]
</script>

<template>
  <div class="characters-page">
    <div class="content">
      <h1 class="page-title">햄토리 친구들</h1>
      <p class="page-subtitle">테스트로 만날 수 있는 캐릭터들이에요!</p>

      <div class="character-list">
        <div
          v-for="char in characters"
          :key="char.name"
          class="character-card"
          :style="{ borderColor: char.color }"
        >
          <div class="card-header">
            <span class="char-emoji">{{ char.emoji }}</span>
            <div class="char-info">
              <h3 class="char-name" :style="{ color: char.color }">{{ char.name }}</h3>
              <p class="char-title">{{ char.title }}</p>
            </div>
          </div>
          <p class="char-description">{{ char.description }}</p>
          <ul class="char-traits">
            <li v-for="(trait, idx) in char.traits" :key="idx">{{ trait }}</li>
          </ul>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.characters-page {
  min-height: calc(100vh - 52px);
  padding: 20px 40px 40px;
  background: linear-gradient(180deg, #FFF9F0 0%, #FFE8D0 100%);
}

.content {
  max-width: 1200px;
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

.character-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 20px;
}

.character-card {
  background: white;
  border-radius: 20px;
  padding: 24px;
  border: 2px solid;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.05);
}

.card-header {
  display: flex;
  align-items: center;
  gap: 16px;
  margin-bottom: 16px;
}

.char-emoji {
  font-size: 48px;
}

.char-info {
  flex: 1;
}

.char-name {
  font-size: 22px;
  font-weight: 800;
  margin-bottom: 4px;
}

.char-title {
  font-size: 13px;
  color: #666;
  font-weight: 500;
}

.char-description {
  font-size: 14px;
  color: #555;
  line-height: 1.5;
  margin-bottom: 16px;
}

.char-traits {
  list-style: none;
  padding: 0;
  margin: 0;
  background: #F9F9F9;
  border-radius: 12px;
  padding: 12px 16px;
}

.char-traits li {
  font-size: 13px;
  color: #666;
  padding: 6px 0;
  border-bottom: 1px solid #EEE;
}

.char-traits li:last-child {
  border-bottom: none;
}

.char-traits li::before {
  content: '• ';
  color: #FF8C42;
}

@media (max-width: 480px) {
  .characters-page {
    padding: 20px 20px 40px;
  }

  .page-title {
    font-size: 24px;
  }

  .char-emoji {
    font-size: 40px;
  }

  .char-name {
    font-size: 18px;
  }
}
</style>
