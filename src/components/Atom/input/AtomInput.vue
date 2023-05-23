<template>
  <input :type="inputType" :value="inputValue" />
</template>

<script>
export default {
  name: "AtomInput",
  props:{
    inputType: {
      type: String,
      default: "text",
    },
    inputValue: {
      type: String,
      default: "",
    },
  },
};
</script>
