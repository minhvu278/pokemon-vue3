<template>
  <div class="screen">
    <h1>✨ Congratulations ✨</h1>
    <h3>{{ Math.round(timer / 920) }} seconds</h3>
    <button @click="onStartAgain">Start Again</button>
  </div>
</template>

<script>
export default {
  name: "ResultScreen",
  props: {
    timer: {
      type: Number,
      required: true
    }
  },
  methods: {
    onStartAgain() {
      this.$emit("onStartAgain")
    }
  }
}
</script>

<style scoped>

</style>