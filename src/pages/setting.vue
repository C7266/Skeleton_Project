<script setup>
import { ref, computed, onMounted } from 'vue';
import axios from 'axios';

import Profile from '@/components/Setting/Profile.vue';
import FixedList from '@/components/Setting/FixedList.vue';
import CategorySelector from '@/components/Setting/CategoryList.vue';

// ── 카테고리 ──────────────────────────────────
const categories = ref([]);
const selectedCategory = ref(null);

const fetchCategories = async () => {
  const res = await axios.get('/api/categories');
  categories.value = res.data;
};

// ── 고정 수입/지출 ─────────────────────────────
const transactions = ref([]);

const fetchTransactions = async () => {
  const res = await axios.get('/api/transactions');
  transactions.value = res.data;
};

const fixedIncome = computed(() =>
  transactions.value.filter((v) => v.fix === true && v.type === 'income'),
);

const fixedExpense = computed(() =>
  transactions.value.filter((v) => v.fix === true && v.type === 'expense'),
);

const deleteFixed = async (id) => {
  await axios.patch(`/api/transactions/${id}`, { fix: false });
  transactions.value = transactions.value.map((v) =>
    v.id === id ? { ...v, fix: false } : v,
  );
};

onMounted(async () => {
  await fetchCategories();
  await fetchTransactions();
});
</script>

<template>
  <div class="setting-page">
    <!-- 개인 설정 카드 -->
    <Profile />

    <!-- 사용자 설정 카드 -->
    <section class="card">
      <p class="card-label">사용자 설정</p>

      <!-- 카테고리 수정 -->
      <div class="section-block">
        <h3 class="section-title">카테고리 수정</h3>
        <CategorySelector
          :categories="categories"
          :editable="true"
          v-model="selectedCategory"
        />
      </div>

      <!-- ✅ 수입 먼저, 지출 나중 -->
      <div class="fixed-wrapper">
        <FixedList
          title="저장된 고정 수입"
          type="income"
          :items="fixedIncome"
          @delete="deleteFixed"
        />
        <FixedList
          title="저장된 고정 지출"
          type="expense"
          :items="fixedExpense"
          @delete="deleteFixed"
        />
      </div>
    </section>
  </div>
</template>

<style scoped>
.setting-page {
  display: flex;
  flex-direction: column;
  gap: 20px;
  padding: 24px;
  min-height: 100%;
}

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

.section-block {
  margin: 0 24px;
  padding: 0 0 2rem 0;
}

.section-title {
  font-size: 18px;
  font-weight: 700;
  color: #1a1a1a;
  margin: 0 0 20px;
}

/* 고정 수입/지출 2열 */
.fixed-wrapper {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0 60px;
  margin: 0 24px;
  padding: 0 0 2rem 0;
}

/* 모바일 */
@media (max-width: 991px) {
  .setting-page {
    padding: 16px; /* 패딩 줄임 */
  }

  .card {
    padding: 16px; /* 카드 패딩 줄임 */
  }

  .section-block {
    margin: 0; /* margin 제거 */
  }

  .category-grid {
    grid-template-columns: 1fr !important;
  }

  /* 1열로 변경 */
  .fixed-wrapper {
    grid-template-columns: 1fr !important;
    gap: 24px 0; /* 수입/지출 사이 간격 */
    margin: 0; /* margin 제거 */
  }
}
</style>
