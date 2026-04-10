<template>
  <table class="table table-hover align-middle mb-0">
    <thead>
      <tr class="border-bottom border-2">
        <th class="text-secondary fw-normal fs-6 py-2" style="width: 120px">
          날짜
        </th>
        <th class="text-secondary fw-normal fs-6 py-2 text-center">내용</th>

        <!-- 모바일 숨김 -->
        <th
          class="text-secondary fw-normal fs-6 py-2 text-center d-none d-lg-table-cell"
          style="width: 120px"
        >
          카테고리
        </th>
        <th
          class="text-secondary fw-normal fs-6 py-2 text-center"
          style="width: 150px"
        >
          금액
        </th>
        <!-- 모바일 숨김 -->
        <th
          class="text-secondary fw-normal fs-6 py-2 text-center d-none d-lg-table-cell"
          style="width: 100px"
        >
          고정
        </th>
        <!-- 모바일 숨김 -->
        <th
          class="text-secondary fw-normal fs-6 py-2 text-center d-none d-lg-table-cell"
          style="width: 90px"
        >
          구분
        </th>
        <th
          class="text-secondary fw-normal fs-6 py-2 text-center"
          style="width: 130px"
        >
          관리
        </th>
      </tr>
    </thead>

    <tbody>
      <tr v-for="t in transactions" :key="t.id">
        <td class="text-muted small">{{ formatDate(t.date) }}</td>

        <td>
          <div class="fw-semibold">{{ t.title }}</div>
          <div v-if="t.memo" class="text-muted" style="font-size: 12px">
            {{ t.memo }}
          </div>
        </td>

        <!-- 모바일 숨김 -->
        <td class="text-center fs-6 d-none d-lg-table-cell">
          {{ getCategory(t.categoryId)?.name }}
        </td>

        <td class="text-center fw-semibold">
          <span :class="t.type === 'expense' ? 'text-danger' : 'text-success'">
            {{ t.type === 'expense' ? '-' : '+' }}{{ formatAmount(t.amount) }}
          </span>
        </td>

        <!-- 모바일 숨김 -->
        <td class="text-center d-none d-lg-table-cell">
          <span
            v-if="t.fix"
            class="badge rounded-pill bg-secondary-subtle text-secondary"
          >
            고정
          </span>
        </td>

        <!-- 모바일 숨김 -->
        <td class="text-center d-none d-lg-table-cell">
          <span
            class="fs-6"
            :class="t.type === 'income' ? 'text-success' : 'text-danger'"
          >
            {{ t.type === 'income' ? '수입' : '지출' }}
          </span>
        </td>

        <td class="text-center">
          <button class="btn btn-mint me-2" @click="$emit('edit', t.id)">
            수정
          </button>
          <button class="btn btn-red" @click="$emit('delete', t.id)">
            삭제
          </button>
        </td>
      </tr>

      <tr v-if="!transactions.length">
        <td colspan="7" class="text-center text-muted py-5">
          거래 내역이 없습니다.
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script setup>
defineProps({
  transactions: {
    type: Array,
    default: () => [],
  },
  getCategory: {
    type: Function,
    required: true,
  },
  formatDate: {
    type: Function,
    required: true,
  },
  formatAmount: {
    type: Function,
    required: true,
  },
});

defineEmits(['edit', 'delete']);
</script>

<style scoped>
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

/* 모바일에서 내용 컬럼 줄바꿈 방지 */
@media (max-width: 991px) {
  table {
    table-layout: fixed; /* 컬럼 너비 고정 */
    width: 100%;
  }

  /* 날짜 컬럼 */
  td:first-child,
  th:first-child {
    width: 85px;
    font-size: 11px;
    white-space: nowrap;
  }

  /* 내용 컬럼 */
  td:nth-child(2),
  th:nth-child(2) {
    width: auto;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    max-width: 100px;
  }

  /* 금액 컬럼 */
  td:nth-child(3),
  th:nth-child(3) {
    width: 80px;
    font-size: 12px;
    white-space: nowrap;
  }

  /* 관리 컬럼 */
  td:last-child,
  th:last-child {
    width: 90px;
  }

  /* 버튼 작게 */
  .btn {
    padding: 2px 6px;
    font-size: 11px;
    margin: 1px !important;
  }
}
</style>
