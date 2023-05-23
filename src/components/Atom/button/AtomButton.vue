<template>
  <button :type="btnType" :name="btnName" @click="clickBtn"><slot></slot></button>
</template>

<script>
export default {
  name: "AtomButton",
  props: {
    btnType: {
      type: String,
      default: "button",
    },
    btnName: {
      type: String,
      default: "btnName",
    },
  },
  methods: {
    clickBtn: function (event) {
      this.$emit("click-btn",event);
    },
  },
};
</script>

<style>

</style>
