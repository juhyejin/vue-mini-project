<script setup lang="ts">
import AtomButton from "@/components/Atom/button/AtomButton.vue";
import { onMounted, reactive, ref } from "vue";
import NewScheduleModal from "@/components/module/NewScheduleModal.vue";

const getDateInfo: Date = new Date();
const weekDay: ReadonlyArray<string> = [
  "일",
  "월",
  "화",
  "수",
  "목",
  "금",
  "토",
];
const isShowNewScheduleModal = ref(false);

const currentDate = ref<string>(
  getDateInfo.toLocaleDateString("ko-KR", {
    year: "numeric",
    month: "numeric",
    day: "numeric",
  })
);
interface yearMonthDayType {
  year: number;
  month: number;
  day: number;
}
const currentYearMonthDay= reactive<yearMonthDayType>({
  year: getDateInfo.getFullYear(),
  month: getDateInfo.getMonth(),
  day: getDateInfo.getDay(),
});
interface scheduleType {
  time: string;
  detail: string;
}
interface dayInfoType {
  fullDay: string;
  schedule?: scheduleType[];
}
interface dayOfMonthType {
  day?: number | null;
  dayInfo?: dayInfoType;
}

const dayOfMonth = ref<dayOfMonthType[]>([]);
const daysOfMonth = (year: number, month: number): void => {
  const startDayOfMonth = new Date(year, month, 1).getDay();
  const lastDateOfMonth = new Date(year, month + 1, 0).getDate();
  repeatAddCurrentDays(startDayOfMonth, null);
  repeatAddCurrentDays(lastDateOfMonth);
  const currentDaysLength = dayOfMonth.value.length;
  if (currentDaysLength % 7 !== 0) {
    repeatAddCurrentDays(7 - (currentDaysLength % 7), null);
  }
};
const repeatAddCurrentDays = (untilNum: number, value?: null): void => {
  if (value !== null) {
    for (let i = 0; i < untilNum; i++) {
      dayOfMonth.value.push({
        day: i + 1,
        dayInfo: {
          fullDay: `${currentYearMonthDay.year}. ${currentYearMonthDay.month+1}. ${i + 1}.`,
        },
      });
    }
  } else {
    for (let i = 0; i < untilNum; i++) {
      dayOfMonth.value.push({
        day: null,
      });
    }
  }
};
const changeMonth = (val: number): void => {
  getDateInfo.setMonth(getDateInfo.getMonth() + val);
  currentYearMonthDay.month = getDateInfo.getMonth();
  currentYearMonthDay.year = getDateInfo.getFullYear();
  dayOfMonth.value = [];
  daysOfMonth(currentYearMonthDay.year, currentYearMonthDay.month);
};
// vue 생명주기
onMounted(() => {
  daysOfMonth(currentYearMonthDay.year, currentYearMonthDay.month);
});
</script>

<template>
  <main class="calender-container">
    <div class="calender-header">
      <AtomButton @click-btn="changeMonth(-1)">이전달</AtomButton>
      <div class="calender-title">
        {{ currentYearMonthDay.year }}년 {{ currentYearMonthDay.month + 1 }}월
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
          :class="{ currentDay: currentDate === day.dayInfo?.fullDay }"
          v-for="(day, idx) in dayOfMonth"
          :key="idx"
          @click="isShowNewScheduleModal = !isShowNewScheduleModal"
        >
          {{ day.day }}
        </div>
      </section>
    </div>
    <NewScheduleModal :is-show="isShowNewScheduleModal" @click-close="(value)=> isShowNewScheduleModal = value"/>
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
  padding: 10px;
}
.currentDay{
  position: relative;
  color: #fff;
  z-index: 1;
}
.currentDay::before {
  z-index: -1;
  width: 25px;
  height: 25px;
  background: #4fb995;
  display: block;
  position: absolute;
  top: 10px;
  left: 1px;
  content: "";
  border-radius: 50%;
}


@media (max-width: 520px) {
  .days-container {
    grid-auto-rows: 0.4fr;
  }
}
</style>
