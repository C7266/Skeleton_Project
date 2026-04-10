<template>
  <div class="topbar">
    <!-- 햄버거 버튼: lg 이하에서만 보임 -->
    <button class="d-lg-none hamburger" @click="$emit('toggle-sidebar')">
      ☰
    </button>
    <div class="topbar">
      <span class="topbar-title">{{ title }}</span>
    </div>
  </div>
</template>

<script setup>
defineProps({
  title: {
    type: String,
    default: '',
  },
});
defineEmits(['toggle-sidebar']);
</script>

<style scoped>
.topbar {
  width: 100%;
  height: 56px;
  background: #fff;
  border-bottom: 1px solid #e0e0e0;
  display: flex;
  align-items: center;
  padding: 2rem 28px;
}

.topbar-title {
  font-size: 18px;
  font-weight: 700;
  color: #1a1a1a;
}
</style>
