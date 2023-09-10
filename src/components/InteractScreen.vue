<template>
  <div class="screen">
    <h1>Interact here ...</h1>
    <card-flip v-for="(card, index) in cardsContext" :key="index" :imgBackFaceUrl="`images/${card}.png`" />
  </div>
</template>

<script>
import CardFlip from "./CardItem"

export default {
  name: "InteractScreen",
  components: {
    CardFlip
  },
  props: {
    cardsContext: {
      type: Array,
      default: function () {
        return [];
      }
    }
  }
}
</script>

<style scoped>

</style>