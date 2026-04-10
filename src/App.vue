<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue';
import { useRoute } from 'vue-router';
import Header from './components/layout/Header.vue';
import Sidebar from './components/layout/Sidebar.vue';
import TopBar from './components/layout/TopBar.vue';

const route = useRoute();

const isSidebarOpen = ref(false);
const toggleSidebar = () => {
  isSidebarOpen.value = !isSidebarOpen.value;
};
const closeSidebar = () => {
  isSidebarOpen.value = false;
};

const windowWidth = ref(window.innerWidth);
const isDesktop = computed(() => windowWidth.value >= 992);

const updateWidth = () => {
  windowWidth.value = window.innerWidth;
};
onMounted(() => window.addEventListener('resize', updateWidth));
onUnmounted(() => window.removeEventListener('resize', updateWidth));
</script>

<template>
  <div class="app-layout">
    <!-- 오버레이: 모바일에서 사이드바 열렸을 때 -->
    <div
      v-if="!isDesktop && isSidebarOpen"
      class="overlay"
      @click="closeSidebar"
    />

    <!-- Sidebar에 직접 클래스 바인딩 -->
    <Sidebar
      :class="
        !isDesktop && !isSidebarOpen
          ? 'sidebar-hidden'
          : !isDesktop && isSidebarOpen
            ? 'sidebar-open'
            : ''
      "
      @close="closeSidebar"
    />

    <div class="right-panel">
      <TopBar
        v-if="route.name === 'setting'"
        title="설정"
        @toggle-sidebar="toggleSidebar"
      />
      <Header v-else @toggle-sidebar="toggleSidebar" />

      <main class="content">
        <RouterView />
      </main>
    </div>
  </div>
</template>

<style scoped>
.app-layout {
  display: flex;
  min-height: 100vh;
}

.right-panel {
  display: flex;
  flex-direction: column;
  flex: 1;
  overflow: hidden;
}

.content {
  flex: 1;
  overflow-y: auto;
  background: #f5f4f0;
}

.overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.4);
  z-index: 999;
}

/* 모바일: 열린 상태 */
.sidebar-open {
  position: fixed !important;
  left: 0;
  top: 0;
  height: 100vh;
  z-index: 1000;
  transform: translateX(0);
  transition: transform 0.3s ease;
}

/* 모바일: 닫힌 상태 */
.sidebar-hidden {
  position: fixed !important;
  left: 0;
  top: 0;
  height: 100vh;
  z-index: 1000;
  transform: translateX(-100%);
  transition: transform 0.3s ease;
}
</style>
