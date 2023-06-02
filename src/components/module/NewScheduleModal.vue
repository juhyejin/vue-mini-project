<script setup lang="ts">
import CloseIcon from "@/components/icons/CloseIcon.vue";
import AtomInput from "@/components/Atom/input/AtomInput.vue";
import AtomButton from "@/components/Atom/button/AtomButton.vue";
import type { PropType } from "vue";
import { reactive } from "vue";

const props = defineProps({
  isShow: Boolean,
  currentDay: {
    type: String as PropType<string | undefined>,
  },
});
interface scheduleType {
  startTime?: string;
  endTime?: string;
  detail: string;
  color: string;
}
const scheduleInfo = reactive<scheduleType>({
  startTime: "",
  endTime: "",
  detail: "",
  color: "",
});

const emit = defineEmits(["click-close", "submit-schedule-data"]);
const clickClose = (): void => {
  scheduleInfo.startTime = "";
  scheduleInfo.endTime = "";
  scheduleInfo.detail = "";
  emit("click-close", false);
};
const clickOutCard = (): void => {
  clickClose();
};
const clickInCard = (event: MouseEvent): void => {
  event.stopPropagation();
};
const submitSchedule = (event: Event): void => {
  event.preventDefault();
  const { startTime, endTime, detail, color } = scheduleInfo;
  emit("submit-schedule-data", {
    fullDay: props.currentDay,
    startTime,
    endTime,
    detail,
    color,
  });
  clickClose();
};
</script>

<template>
  <div class="main-container" v-if="isShow" @click="clickOutCard">
    <div class="card" @click="clickInCard">
      <div class="close" @click="clickClose">
        <CloseIcon style="width: 20px; height: 20px" />
      </div>
      <form class="contents" @submit="submitSchedule">
        <div>
          <div class="title">
            {{ currentDay }}
          </div>
          <AtomInput
            type="time"
            name="startTime"
            v-model="scheduleInfo.startTime"
            @input.native="scheduleInfo.startTime = $event.target.value"
          />
          <AtomInput
            type="time"
            :min="scheduleInfo.startTime"
            name="endTime"
            v-model="scheduleInfo.endTime"
            @input.native="scheduleInfo.endTime = $event.target.value"
          />
          <AtomInput
            placeholder="일정을 입력해주세요"
            required
            style="width: 100%"
            v-model="scheduleInfo.detail"
            @input.native="scheduleInfo.detail = $event.target.value"
          />
          <AtomInput
            input-type="color"
            :model-value="scheduleInfo.color"
            @input.native="scheduleInfo.color = $event.target.value"
          />
        </div>
        <AtomButton btn-type="submit" class="btn">확인</AtomButton>
      </form>
    </div>
  </div>
</template>

<style scoped>
.main-container {
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.1);
  z-index: 2;
}
.main-container .card {
  width: 250px;
  height: 250px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background: #fff;
  border-radius: 10px;
  padding: 20px;
  box-shadow: 1px 2px 5px 2px rgba(0, 0, 0, 0.3);
  display: flex;
  flex-direction: column;
}
.main-container .card .close {
  cursor: pointer;
  width: fit-content;
  place-self: end;
}
.main-container .card .title {
  font-size: 18px;
  font-weight: bold;
  text-align: center;
}
.main-container .card .contents {
  flex-grow: 1;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.btn {
  background: #0c75ff;
  color: #fff;
  height: 30px;
  outline: 0;
  border: 0;
  border-radius: 8px;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.2);
  cursor: pointer;
  font-weight: bold;
  transition: 0.5s;
}
.btn:hover,
.btn:active {
  background: #064597;
}
</style>
