<script setup>
defineProps({
  currentPage: String
})

const emit = defineEmits(['navigate'])

const navItems = [
  { id: 'home', label: 'Home' },
  { id: 'test', label: '테스트하기' },
  { id: 'characters', label: '전체 캐릭터' }
]
</script>

<template>
  <nav class="navbar">
    <div class="nav-items">
      <button
        v-for="item in navItems"
        :key="item.id"
        class="nav-item"
        :class="{ active: currentPage === item.id }"
        @click="emit('navigate', item.id)"
      >
        {{ item.label }}
      </button>
    </div>
  </nav>
</template>

<style scoped>
.navbar {
  background: transparent;
}

.nav-items {
  display: flex;
  justify-content: center;
  max-width: 500px;
  margin: 0 auto;
}

.nav-item {
  flex: 1;
  padding: 16px 12px;
  font-size: 15px;
  font-weight: 600;
  color: #C4A77D;
  background: none;
  border: none;
  border-bottom: 3px solid transparent;
  cursor: pointer;
  transition: all 0.2s ease;
}

.nav-item:hover {
  color: #FF8C42;
}

.nav-item.active {
  color: #FF8C42;
  border-bottom-color: #FF8C42;
}
</style>
