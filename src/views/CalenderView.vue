<script setup lang="ts">
import AtomButton from "@/components/Atom/button/AtomButton.vue";
import { onMounted, reactive, ref } from "vue";
import NewScheduleModal from "@/components/module/NewScheduleModal.vue";
import CloseIcon from "@/components/icons/CloseIcon.vue";

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
  color?: string;
}
interface dayInfoType {
  fullDay: string;
  schedule: scheduleType[];
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
          fullDay: `${currentYearMonthDay.year}. ${
            currentYearMonthDay.month + 1
          }. ${i + 1}.`,
          schedule: [],
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
const propsCurrentDay = ref<string | undefined>("");
const clickDay = (dayData: dayOfMonthType): void => {
  const { day, dayInfo } = dayData;
  if (day !== null) {
    propsCurrentDay.value = dayInfo?.fullDay;
    isShowNewScheduleModal.value = true;
  }
};

interface propsScheduleType {
  fullDay: string;
  startTime?: string;
  endTime?: string;
  detail: string;
  color: string;
}
const handleSubmitScheduleData = (scheduleData: propsScheduleType):void => {
  const index = dayOfMonth.value.findIndex((x)=> x.dayInfo?.fullDay === scheduleData.fullDay);
  dayOfMonth.value[index].dayInfo?.schedule?.push({
    time: `${scheduleData.startTime}~${scheduleData.endTime}`,
    detail: scheduleData.detail,
    color: scheduleData.color,
  });
};
const deleteSchedule = (
  event: MouseEvent,
  dayData: dayOfMonthType,
  index: number
) => {
  event.stopPropagation();
  dayData.dayInfo?.schedule.splice(index, 1);
};
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
          @click="clickDay(day)"
        >
          {{ day.day }}
          <ul
            class="scheduleStyle"
            v-if="day.dayInfo?.schedule.length !== 0 && day.dayInfo?.schedule"
          >
            <li
              v-for="(schedule, ids) in day.dayInfo.schedule"
              :key="ids"
              :style="{ background: schedule.color }"
              @click="(event) => event.stopPropagation()"
            >
              <p class="time" v-if="schedule.time.length !== 1">
                {{ schedule.time }}
              </p>
              {{ schedule.detail }}
              <div
                class="deleteBtn"
                @click="(event) => deleteSchedule(event, day, ids)"
              >
                <CloseIcon style="width: 20px; height: 20px;"/>
              </div>
            </li>
          </ul>
        </div>
      </section>
    </div>
    <NewScheduleModal
      :is-show="isShowNewScheduleModal"
      :current-day="propsCurrentDay"
      @click-close="(value) => (isShowNewScheduleModal = value)"
      @submit-schedule-data="handleSubmitScheduleData"
    />
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
  background: #fff;
  border-radius: 8px;
  padding: 10px;
  cursor: pointer;
  transition: 0.4s;
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
  background: #0c75ff;
  display: block;
  position: absolute;
  top: 10px;
  left: 1px;
  content: "";
  border-radius: 50%;
}
ul.scheduleStyle {
  color: #000;
  list-style: none;
  padding: 0;
  text-align: center;
}
li{
  max-width: 150px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  border-radius: 3px;
  margin-top: 3px;
  transition: .3s;
  position: relative;
}
li:hover {
  transform: scale(1.1);
}
li p{
  font-size: 12px;
  line-height: 1.5;
}
li:hover .time{
  display: block;
  opacity: 1;
}
li .time{
  display: none;
  opacity: 0;
  transition: 1s;
}
li .deleteBtn{
  display: none;
}
li .deleteBtn{
  height: 20px;
  display: block;
  position: absolute;
  top: 50%;
  right: 0;
  transform: translate(-50%, -50%);
}
@media (max-width: 520px) {
  .days-container {
    grid-auto-rows: 0.4fr;
  }
}
</style>
