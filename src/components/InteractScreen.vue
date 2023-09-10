<template>
  <div class="screen">
    <div class="screen__inner" :style="{
        width: `${((((920 - 16 * 4) / Math.sqrt(cardsContext.length) - 16) * 3) / 4 + 16) * Math.sqrt(cardsContext.length)}px`
      }">
      <card-flip
          v-for="(card, index) in cardsContext"
          :key="index"
          :ref="`card-${index}`"
          :imgBackFaceUrl="`images/${card}.png`"
          :card="{index, value: card}"
          :cardsContext="cardsContext"
          @onFlip="checkRule($event)"
      />
    </div>
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
.screen {
  width: 100%;
  /*height: 100vh;*/
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);
}

.screen__inner {
  width: calc(424px);
  display: flex;
  flex-wrap: wrap;
  margin: 2rem auto;
}
</style>