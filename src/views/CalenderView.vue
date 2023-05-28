<script setup lang="ts">
import AtomButton from "@/components/Atom/button/AtomButton.vue";
import { reactive } from "vue";

const toDate: Date = new Date();
const weekDay: ReadonlyArray<string> = ['일','월','화','수','목','금','토'];
interface currentDay {
  year: number;
  month: number;
  day: number;
  currentDate: number;
  currentDays: (number | null)[];
}
const currentDayInfo: currentDay = reactive({
  year: toDate.getFullYear(),
  month: toDate.getMonth(),
  day: toDate.getDay(),
  currentDate: toDate.getDate(),
  currentDays: [],
});
const getDateOfMonth = (year: number, month: number, date: number): number => {
  const dateOfMonth = new Date(year, month, date);
  return dateOfMonth.getDate();
};
const getDayOfMonth = (year: number, month: number, date: number): number => {
  const dayOfMonth = new Date(year, month, date);
  return dayOfMonth.getDay();
};

const daysOfMonth = (year: number, month: number): void => {
  const startDayOfMonth = getDayOfMonth(year, month, 1);
  const lastDateOfMonth = getDateOfMonth(year, month + 1, 0);
  const daysList: (number|null)[] = Array.from({ length: lastDateOfMonth }, (v, i) => i + 1);
  for (let i = 0; i < startDayOfMonth; i++) {
    daysList.unshift(null);
  }
  daysList.length = 35;
  currentDayInfo.currentDays = daysList;
};
daysOfMonth(currentDayInfo.year, currentDayInfo.month);

const changeMonth = (val: number) => {
  toDate.setMonth(toDate.getMonth() + val);
  currentDayInfo.month = toDate.getMonth();
  currentDayInfo.year = toDate.getFullYear();
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
        <div
          class="days"
          v-for="(day, idx) in currentDayInfo.currentDays"
          :key="idx"
        >
          {{ day }}
        </div>
      </section>
    </div>
  </main>
</template>

<style>
.calender-container{
  background: #f2f2f2;
  color: #181818;
  padding: 10px;
  display: flex;
  flex-direction: column;
  gap: 10px;
}
.calender-container .calender-header{
  display: flex;
  justify-content: center;
  align-self: center;
}
.calender-container .calender-body {
  flex-grow: 1;
}
.calender-container .calender-body .week{
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
.bg-pink{
  background: pink;
}

@media (max-width: 520px) {
  .days-container{
    grid-auto-rows: 0.4fr;
  }
}
</style>
