<script setup lang="ts">
import AtomButton from "@/components/Atom/button/AtomButton.vue";
import { onMounted, reactive, ref } from "vue";

const toDate: Date = new Date();
const weekDay: ReadonlyArray<string> = [
  "일",
  "월",
  "화",
  "수",
  "목",
  "금",
  "토",
];
interface scheduleType {
  time: string;
  detail: string;
}
interface dayType {
  fullDay: string;
  schedule?: scheduleType[];
}
interface currentDayType {
  year: number;
  month: number;
  day: number;
  currentDate: number;
}
interface currentDaysType {
  day?: number | null;
  dayInfo?: dayType;
}
const currentDayInfo = reactive<currentDayType>({
  year: toDate.getFullYear(),
  month: toDate.getMonth(),
  day: toDate.getDay(),
  currentDate: toDate.getDate(),
});
const currentDays = ref<currentDaysType[]>([]);
const repeatAddCurrentDays = (untilNum: number, value?: null): void => {
  if (value !== null) {
    for (let i = 0; i < untilNum; i++) {
      currentDays.value.push({
        day: i + 1,
        dayInfo: {
          fullDay: `${currentDayInfo.year}.${currentDayInfo.month}.${i + 1}`,
        },
      });
    }
  } else {
    for (let i = 0; i < untilNum; i++) {
      currentDays.value.push({
        day: null,
         });
    }
  }
};
const daysOfMonth = (year: number, month: number): void => {
  const startDayOfMonth = new Date(year, month, 1).getDay();
  const lastDateOfMonth = new Date(year, month + 1, 0).getDate();
  repeatAddCurrentDays(startDayOfMonth, null);
  repeatAddCurrentDays(lastDateOfMonth);
  const currentDaysLength = currentDays.value.length;
  if (currentDaysLength % 7 !== 0) {
    repeatAddCurrentDays(7 - (currentDaysLength % 7), null);
  }
};
onMounted(() => {
  daysOfMonth(currentDayInfo.year, currentDayInfo.month);
});

const changeMonth = (val: number): void => {
  toDate.setMonth(toDate.getMonth() + val);
  currentDayInfo.month = toDate.getMonth();
  currentDayInfo.year = toDate.getFullYear();
  currentDays.value = [];
  daysOfMonth(currentDayInfo.year, currentDayInfo.month);
};
</script>

<template>
  <main class="calender-container">
    <div class="calender-header">
      <AtomButton @click-btn="changeMonth(-1)">이전달</AtomButton>
      <div class="calender-title">
        {{ currentDayInfo.year }}년 {{ currentDayInfo.month + 1 }}월
      </div>
      <AtomButton @click-btn="changeMonth(1)">다음달</AtomButton>
    </div>
    <div class="calender-body">
      <section class="week">
        <div v-for="day in weekDay" :key="day">{{ day }}</div>
      </section>
      <section class="days-container">
        <div class="days" v-for="(day, idx) in currentDays" :key="idx">
          {{ day.day }}
        </div>
      </section>
    </div>
  </main>
</template>

<style>
.calender-container {
  background: #f2f2f2;
  color: #181818;
  padding: 10px;
  display: flex;
  flex-direction: column;
  gap: 10px;
}
.calender-container .calender-header {
  display: flex;
  justify-content: center;
  align-self: center;
}
.calender-container .calender-body {
  flex-grow: 1;
}
.calender-container .calender-body .week {
  grid-column: 1/8;
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  text-align: center;
}
.calender-container .calender-body .days-container {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  grid-auto-rows: 15vh;
  gap: 5px;
}
.calender-container .calender-body .days-container .days {
  border: 1px solid #2c3e50;
  border-radius: 8px;
}
.bg-pink {
  background: pink;
}

@media (max-width: 520px) {
  .days-container {
    grid-auto-rows: 0.4fr;
  }
}
</style>
