<script setup>
const emit = defineEmits(['start'])

const handleStart = () => {
  emit('start')
}
</script>

<template>
  <div class="start-page">
    <div class="content">
      <!-- 로고/캐릭터 영역 -->
      <div class="logo-area">
        <div class="character-icon">🐹</div>
        <h1 class="title">햄토리 테스트</h1>
        <p class="subtitle">Hamtori Test</p>
      </div>

      <!-- 설명 영역 -->
      <div class="description-area">
        <h2 class="main-text">나와 닮은 햄토리 캐릭터는?</h2>
        <p class="sub-text">
          10가지 질문으로 알아보는 나의 햄토리 유형!<br>
          당신은 어떤 햄토리 친구와 닮았을까요?
        </p>
      </div>

      <!-- 시작 버튼 -->
      <button class="start-button" @click="handleStart">
        시작하기
      </button>

      <!-- 푸터 -->
      <p class="footer-text">
        총 12문항 · 약 2분 소요
      </p>
    </div>
  </div>
</template>

<style scoped>
.start-page {
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
  text-align: center;
  padding: 40px 20px;
}

/* 로고 영역 */
.logo-area {
  margin-bottom: 40px;
}

.character-icon {
  font-size: 80px;
  margin-bottom: 16px;
  animation: bounce 2s infinite;
}

@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
}

.title {
  font-size: 36px;
  font-weight: 800;
  color: #FF8C42;
  margin-bottom: 8px;
}

.subtitle {
  font-size: 16px;
  color: #C4A77D;
  font-weight: 500;
  letter-spacing: 2px;
}

/* 설명 영역 */
.description-area {
  margin-bottom: 40px;
}

.main-text {
  font-size: 22px;
  font-weight: 700;
  color: #4A4A4A;
  margin-bottom: 16px;
}

.sub-text {
  font-size: 15px;
  color: #777;
  line-height: 1.6;
}

/* 시작 버튼 */
.start-button {
  width: 100%;
  max-width: 320px;
  padding: 18px 40px;
  font-size: 18px;
  font-weight: 700;
  color: white;
  background: linear-gradient(135deg, #FF8C42 0%, #FF6B35 100%);
  border: none;
  border-radius: 50px;
  cursor: pointer;
  transition: all 0.3s ease;
  box-shadow: 0 4px 15px rgba(255, 140, 66, 0.4);
}

.start-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 6px 20px rgba(255, 140, 66, 0.5);
}

.start-button:active {
  transform: translateY(0);
}

/* 푸터 */
.footer-text {
  margin-top: 24px;
  font-size: 13px;
  color: #AAA;
}

/* 모바일 대응 */
@media (max-width: 480px) {
  .title {
    font-size: 28px;
  }

  .main-text {
    font-size: 18px;
  }

  .character-icon {
    font-size: 60px;
  }
}
</style>
