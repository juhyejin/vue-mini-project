<template>
  <input :type="inputType" :value="inputValue" />
</template>

<script setup lang="ts">
defineProps({
  inputType:{
    type: String,
    default: "text",
  },
  inputValue: {
    type: String,
    default: "",
  },
});
</script>
