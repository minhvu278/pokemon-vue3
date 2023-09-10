<template>
  <main-screen v-if="statusMatch === 'default'" @onStart="onHandleBeforeStart($event)"/>
  <interact-screen
      v-if="statusMatch === 'match'"
      :cardsContext="settings.cardsContext"
      @onFinish="onGetResult"
  />
  <result-screen v-if="statusMatch === 'result'" :timer="timer" @onStartAgain="statusMatch = 'default'" />
</template>

<script>
import MainScreen from "@/components/MainScreen";
import InteractScreen from "@/components/InteractScreen";
import ResultScreen from "@/components/ResultScreen";
import { shuffled } from "./utils/array.js"

export default {
  name: 'App',
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null,
        timer: 0
      },
      statusMatch: "default"
    }
  },
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
  },
  methods: {
    onHandleBeforeStart(config) {
      this.settings.totalOfBlocks = config.totalOfBlocks

      const firstCards = Array.from({length: this.settings.totalOfBlocks / 2}, (_, i) => i + 1);

      const secondCards = [...firstCards]
      const cards = [...firstCards, ...secondCards];
      this.settings.cardsContext = shuffled(shuffled(shuffled(shuffled(cards))))
      this.settings.startedAt = new Date().getTime()

      // Data ready
      this.statusMatch = "match"
    },
    onGetResult() {
      this.timer = new Date().getTime() - this.settings.startedAt

      this.statusMatch = "result"
    }
  },
}
</script>
