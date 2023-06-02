<script setup lang="ts">
defineProps({
  inputType: {
    type: String,
    default: "text",
  },
  modelValue: String,
});
</script>

<template>
  <input :type="inputType" :value="modelValue" />
</template>
