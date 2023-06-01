<script setup lang="ts">
import CloseIcon from "@/components/icons/CloseIcon.vue";
import AtomInput from "@/components/Atom/input/AtomInput.vue";
import AtomButton from "@/components/Atom/button/AtomButton.vue";

defineProps({
  isShow: Boolean,
});
const emit = defineEmits(["click-close"]);
const clickClose = (): void => {
  emit("click-close", false);
};
const clickOutCard = (): void => {
  clickClose();
};
const clickInCard = (event: MouseEvent) :void => {
  event.stopPropagation();
}
const submitSchedule = (event: SubmitEvent): void => {
  event.preventDefault();

}
</script>

<template>
  <div class="main-container" v-if="isShow" @click="clickOutCard">
    <div class="card" @click="clickInCard">
      <div class="close" @click="clickClose">
        <CloseIcon style="width: 20px; height: 20px;" />
      </div>
      <form class="contents" @submit="submitSchedule">
        <AtomInput input-type="time" />
        <AtomInput input-type="time" />
        <AtomInput placeholder="일정을 입력해주세요" required />
        <AtomButton btn-type="submit">확인</AtomButton>
      </form>
    </div>
  </div>
</template>

<style scoped>
.main-container{
  width: 100%;
  height: 100%;
  position: fixed;
  top: 0;
  left: 0;
  background: rgba(0,0,0, 0.1);
  z-index: 2;
}
.main-container .card{
  width: 250px;
  height: 250px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%,-50%);
  background: #fff;
  border-radius: 10px;
  padding: 20px;
  box-shadow: 1px 2px 5px 2px rgba(0,0,0, .3);
  display: flex;
  flex-direction: column;

}
.main-container .card .close{
  cursor: pointer;
  width: fit-content;
  place-self: end;
}
.main-container .card .contents{
  flex-grow: 1;
}
</style>
