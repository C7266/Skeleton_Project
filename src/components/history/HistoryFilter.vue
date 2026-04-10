<template>
  <!-- 필터 영역 -->
  <div class="bg-white rounded-3 shadow-sm p-3 mb-3">
    <div class="d-flex flex-wrap align-items-center gap-2">
      <!-- 기간 -->
      <div class="d-flex align-items-center gap-1 filter-group">
        <label class="fw-semibold text-secondary small mb-0 d-none d-lg-block"
          >기간</label
        >
        <input
          type="date"
          class="form-control form-control-sm filter-date"
          :value="filters.startDate"
          :max="filters.endDate"
          @input="update('startDate', $event.target.value)"
        />
        <span class="text-secondary small">~</span>
        <input
          type="date"
          class="form-control form-control-sm filter-date"
          :value="filters.endDate"
          :min="filters.startDate"
          @input="update('endDate', $event.target.value)"
        />
      </div>

      <!-- 구분 -->
      <div class="d-flex align-items-center gap-1 filter-group">
        <label class="fw-semibold text-secondary small mb-0 d-none d-lg-block"
          >구분</label
        >
        <select
          class="form-select form-select-sm filter-select"
          :value="filters.type"
          @change="update('type', $event.target.value)"
        >
          <option value="">전체</option>
          <option value="income">수입</option>
          <option value="expense">지출</option>
        </select>
      </div>

      <!-- 카테고리 -->
      <div class="d-flex align-items-center gap-1 filter-group">
        <label class="fw-semibold text-secondary small mb-0 d-none d-lg-block"
          >카테고리</label
        >
        <select
          class="form-select form-select-sm filter-select"
          :value="filters.categoryId"
          @change="update('categoryId', $event.target.value)"
        >
          <option value="">카테고리</option>
          <option v-for="c in categories" :key="c.id" :value="c.id">
            {{ c.name }}
          </option>
        </select>
      </div>
    </div>

    <!-- 메모 검색 -->
    <div class="input-group mt-2">
      <span class="input-group-text bg-white border-end-0">
        <i class="bi bi-search text-secondary" />
      </span>
      <input
        type="text"
        class="form-control form-control-sm border-start-0"
        placeholder="메모 검색..."
        :value="filters.keyword"
        @input="update('keyword', $event.target.value)"
      />
    </div>
  </div>

  <!-- 수입/지출 요약 카드 -->
  <div class="row g-2 mb-3">
    <div class="col-6">
      <div
        class="bg-white rounded-3 shadow-sm p-2 p-lg-3 d-flex align-items-center gap-2"
      >
        <i class="bi bi-coin text-success summary-icon"></i>
        <div>
          <div class="text-secondary summary-label">수입</div>
          <div class="fw-bold text-success summary-amount">
            {{ formatAmount(totalIncome) }}원
          </div>
        </div>
      </div>
    </div>
    <div class="col-6">
      <div
        class="bg-white rounded-3 shadow-sm p-2 p-lg-3 d-flex align-items-center gap-2"
      >
        <i class="bi bi-coin text-danger summary-icon"></i>
        <div>
          <div class="text-secondary summary-label">지출</div>
          <div class="fw-bold text-danger summary-amount">
            {{ formatAmount(totalExpense) }}원
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
const props = defineProps({
  filters: { type: Object, required: true },
  categories: { type: Array, default: () => [] },
  totalIncome: { type: Number, default: 0 },
  totalExpense: { type: Number, default: 0 },
});

const formatAmount = (amount) => amount.toLocaleString();
const emit = defineEmits(['update:filters']);
const update = (key, value) => {
  emit('update:filters', { ...props.filters, [key]: value });
};
</script>

<style scoped>
/* 날짜 input */
.filter-date {
  width: 130px;
}

/* select */
.filter-select {
  width: 90px;
}

/* 수입/지출 요약 */
.summary-icon {
  font-size: 20px;
}

.summary-label {
  font-size: 11px;
  color: #888;
}

.summary-amount {
  font-size: 14px;
  font-weight: 700;
}

/* 모바일 */
@media (max-width: 991px) {
  .filter-date {
    width: 110px; /* 날짜 더 좁게 */
    font-size: 11px;
    padding: 2px 4px;
  }

  .filter-select {
    width: 75px; /* select 더 좁게 */
    font-size: 11px;
    padding: 2px 4px;
  }

  .filter-group {
    flex-shrink: 0;
  }

  .summary-icon {
    font-size: 16px;
  }

  .summary-amount {
    font-size: 13px;
  }
}
</style>
