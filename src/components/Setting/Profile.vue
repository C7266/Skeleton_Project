<template>
  <section class="card">
    <p class="card-label">개인 설정</p>

    <div class="profile-body">
      <!-- 아바타 -->
      <div class="avatar">
        {{ user?.name?.[0] || ' ' }}
      </div>

      <!-- 이름 영역 -->
      <div class="profile-info">
        <!-- 이름 라벨 + 버튼 한 줄 -->
        <div class="label-row">
          <span class="info-label">이름</span>

          <!-- 일반 상태: 수정 버튼 -->
          <template v-if="!isEdit">
            <button class="btn btn-gray btn-sm" @click="startEdit">수정</button>
          </template>

          <!-- 수정 상태: 완료/취소 버튼 -->
          <template v-else>
            <button class="btn btn-mint btn-sm" @click="saveName">완료</button>
            <button class="btn btn-gray btn-sm" @click="cancelEdit">
              취소
            </button>
          </template>
        </div>

        <!-- input은 아래에 단독으로 -->
        <input v-if="!isEdit" class="name-input" :value="user?.name" readonly />
        <input v-else class="name-input" v-model="editName" />
      </div>
    </div>
  </section>
</template>

<script setup>
import { computed, onMounted, ref } from 'vue';
import { useUserStore } from '@/stores/user';

const store = useUserStore();
const user = computed(() => store.user);

const isEdit = ref(false);
const editName = ref('');

onMounted(async () => {
  try {
    await store.fetchUser();
  } catch {
    alert('DB에서 사용자 정보를 불러오지 못했습니다.');
  }
});

const startEdit = () => {
  editName.value = user.value?.name ?? '';
  isEdit.value = true;
};

const cancelEdit = () => {
  isEdit.value = false;
};

const saveName = async () => {
  const trimmedName = editName.value.trim();
  if (!trimmedName) return;
  try {
    await store.updateUser(trimmedName);
    isEdit.value = false;
  } catch {
    alert('DB 저장에 실패했습니다. json-server 상태를 확인해주세요.');
  }
};
</script>

<style scoped>
.card {
  background: #fff;
  border: 1px solid #e0e0e0;
  border-radius: 16px;
  padding: 24px 28px;
}

.card-label {
  font-size: 15px;
  color: #444;
  margin: 0 0 20px;
}

.profile-body {
  display: flex;
  align-items: center;
  gap: 24px;
}

.avatar {
  width: 90px;
  height: 90px;
  border-radius: 50%;
  background: #e2a000;
  color: #fff;
  font-size: 36px;
  font-weight: bold;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.profile-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 8px;
}

/* 이름 라벨 + 버튼 한 줄 */
.label-row {
  display: flex;
  align-items: center;
  gap: 8px;
}

.info-label {
  font-size: 13px;
  color: #555;
}

/* input 단독 한 줄 */
.name-input {
  width: 100%;
  height: 42px;
  padding: 0 14px;
  border: 1px solid #ddd;
  border-radius: 8px;
  font-size: 16px;
  background: #f5f5f5;
  outline: none;
}

.name-input:not([readonly]) {
  background: #fff;
  border-color: #aaa;
}

/* 버튼 공통 */
.btn {
  border-radius: 8px;
  font-size: 13px;
  font-weight: 500;
  cursor: pointer;
  white-space: nowrap;
  border: 1px solid #ddd;
}

.btn-sm {
  height: 30px;
  padding: 0 12px;
}

.btn-gray {
  background: #fff;
  color: #333;
}

.btn-gray:hover {
  background: #f0f0f0;
}

.btn-mint {
  background: #d4f0d4;
  color: #2d8a2d;
  border-color: #d4f0d4;
}
</style>
