<template>
  <div class="screen">
    <h1>Interact here ...</h1>
    <card-flip
        v-for="(card, index) in cardsContext"
        :key="index"
        :ref="`card-${index}`"
        :imgBackFaceUrl="`images/${card}.png`"
        :card="{index, value: card}"
        @onFlip="checkRule($event)"
    />
  </div>
</template>

<script>
import CardFlip from "./CardItem"

export default {
  name: "InteractScreen",
  components: {
    CardFlip
  },
  data() {
    return {
      rules: [],
    }
  },
  methods: {
    checkRule(card) {
      if (this.rules.length === 2) return false

      this.rules.push(card)

      if (this.rules.length === 2 && this.rules[0].value === this.rules[1].value) {
        console.log("Right...")
        // Add class disable
        this.$refs[`card-${this.rules[0].index}`][0].enableDisableMode()
        this.$refs[`card-${this.rules[1].index}`][0].enableDisableMode()

        // Reset rules []
        this.rules = []

        const disableElements = document.querySelectorAll(".screen .card.disable")

        if (disableElements && disableElements.length === this.cardsContext.length - 2) {
          setTimeout(() => {
            this.$emit('onFinish')
          }, 920)
        }

      } else if (this.rules.length === 2 && this.rules[0].value !== this.rules[1].value) {
        console.log("Wrong...")
       setTimeout(() => {
         //Close two card
         this.$refs[`card-${this.rules[0].index}`][0].onFlipBackCard()
         this.$refs[`card-${this.rules[1].index}`][0].onFlipBackCard()

         // Reset rules to []
         this.rules = []
       }, 800)
      } else return false;
    },
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