<template>
  <button :type="btnType" :name="btnName" @click="clickBtn"><slot></slot></button>
</template>

<script setup lang="ts">
defineProps({
  btnType: {
    type: String as () => "button" | "submit" | "reset",
    default: "button",
  },
  btnName: {
    type: String,
    default: "btnName",
  },
});
const emit = defineEmits(["click-btn"]);
const clickBtn = (event: MouseEvent): void => {
  emit("click-btn", event);
};
</script>

<style>

</style>
