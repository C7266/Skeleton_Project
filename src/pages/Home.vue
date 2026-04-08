<template>
  <div class="p-4">
    <!-- 월 이동 -->
    <div class="d-flex align-items-center justify-content-center gap-3 mb-3">
      <button class="btn btn-sm btn-outline-secondary" @click="prevMonth">
        &lt;
      </button>
      <span class="fw-bold fs-5"
        >{{ year }}.{{ String(month).padStart(2, '0') }}</span
      >
      <button class="btn btn-sm btn-outline-secondary" @click="nextMonth">
        &gt;
      </button>
    </div>

    <!-- 요일 헤더 -->
    <div class="d-grid mb-1" style="grid-template-columns: repeat(7, 1fr)">
      <span
        v-for="day in days"
        :key="day"
        class="text-center fw-semibold py-2"
        :class="
          day === '일'
            ? 'text-danger'
            : day === '토'
              ? 'text-primary'
              : 'text-secondary'
        "
        style="font-size: 13px"
        >{{ day }}</span
      >
    </div>

    <!-- 날짜 셀 -->
    <div class="d-grid gap-2" style="grid-template-columns: repeat(7, 1fr)">
      <div
        v-for="cell in calendarCells"
        :key="cell.date"
        class="rounded-3 p-2"
        style="min-height: 90px; background: #fff"
        :style="!cell.isCurrentMonth ? 'opacity: 0.35;' : ''"
      >
        <span
          class="fw-semibold"
          style="font-size: 14px"
          :class="!cell.isCurrentMonth ? 'text-secondary' : ''"
          >{{ cell.day }}</span
        >
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const today = new Date();
const year = ref(today.getFullYear());
const month = ref(today.getMonth() + 1);

const days = ['일', '월', '화', '수', '목', '금', '토'];

const calendarCells = computed(() => {
  const firstDay = new Date(year.value, month.value - 1, 1).getDay();
  const lastDate = new Date(year.value, month.value, 0).getDate();
  const prevLastDate = new Date(year.value, month.value - 1, 0).getDate();

  const cells = [];

  // 이전 달 빈 칸
  for (let i = firstDay - 1; i >= 0; i--) {
    cells.push({
      date: `prev-${i}`,
      day: prevLastDate - i,
      isCurrentMonth: false,
    });
  }

  // 현재 달
  for (let d = 1; d <= lastDate; d++) {
    cells.push({
      date: `${year.value}-${month.value}-${d}`,
      day: d,
      isCurrentMonth: true,
    });
  }

  // 다음 달 빈 칸
  const remaining = 42 - cells.length;
  for (let d = 1; d <= remaining; d++) {
    cells.push({ date: `next-${d}`, day: d, isCurrentMonth: false });
  }

  return cells;
});

function prevMonth() {
  if (month.value === 1) {
    year.value--;
    month.value = 12;
  } else month.value--;
}

function nextMonth() {
  if (month.value === 12) {
    year.value++;
    month.value = 1;
  } else month.value++;
}
</script>
