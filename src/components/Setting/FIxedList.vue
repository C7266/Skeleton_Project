<template>
  <div class="fixed-list">
    <div v-for="item in items" :key="item.id" class="list-row">
      <!-- 수정 중 -->
      <template v-if="editingId === item.id">
        <input
          class="edit-input"
          v-model="editingTitle"
          @keyup.enter="saveEdit(item)"
          @keyup.escape="cancelEdit"
        />
        <div class="row-actions">
          <button class="btn btn-mint" @click="saveEdit(item)">완료</button>
          <button class="btn btn-gray" @click="cancelEdit">취소</button>
        </div>
      </template>

      <!-- 일반 상태 -->
      <template v-else>
        <span class="row-title">{{ item.title }}</span>
        <div class="row-actions">
          <button class="btn btn-mint" @click="startEdit(item)">수정</button>
          <button class="btn btn-red" @click="$emit('delete', item.id)">
            삭제
          </button>
        </div>
      </template>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';

defineProps({
  title: String,
  items: Array,
});

const emit = defineEmits(['edit', 'delete']);

const editingId = ref(null);
const editingTitle = ref('');

const startEdit = (item) => {
  editingId.value = item.id;
  editingTitle.value = item.title;
};

const cancelEdit = () => {
  editingId.value = null;
  editingTitle.value = '';
};

const saveEdit = (item) => {
  const trimmed = editingTitle.value.trim();
  if (!trimmed) return;
  emit('edit', { ...item, title: trimmed });
  cancelEdit();
};
</script>

<style scoped>
.fixed-list {
  display: flex;
  flex-direction: column;
  gap: 14px;
}

/* 각 행 */
.list-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 8px;
}

.row-title {
  font-size: 14px;
  color: #333;
  flex: 1;
}

/* 수정 중 input */
.edit-input {
  flex: 1;
  height: 32px;
  padding: 0 10px;
  border: 1px solid #ccc;
  border-radius: 6px;
  font-size: 14px;
  outline: none;
}

.edit-input:focus {
  border-color: #aaa;
}

/* 버튼 묶음 */
.row-actions {
  display: flex;
  gap: 6px;
}

/* 버튼 공통 */
.btn {
  padding: 4px 14px;
  border: none;
  border-radius: 20px;
  font-size: 12px;
  font-weight: 600;
  cursor: pointer;
  white-space: nowrap;
}

.btn-mint {
  background: #d4f0d4;
  color: #2d8a2d;
}
.btn-gray {
  background: #eee;
  color: #555;
}
.btn-red {
  background: #ffd6d6;
  color: #c0392b;
}
</style>
