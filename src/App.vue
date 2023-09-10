<template>
  <main-screen v-if="statusMatch === 'default'" @onStart="onHandleBeforeStart($event)"/>
  <interact-screen v-if="statusMatch === 'match'" :cardsContext="settings.cardsContext" />
</template>

<script>
import MainScreen from "@/components/MainScreen";
import InteractScreen from "@/components/InteractScreen";
import { shuffled } from "./utils/array.js"

export default {
  name: 'App',
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardsContext: [],
        startedAt: null
      },
      statusMatch: "default"
    }
  },
  components: {
    MainScreen,
    InteractScreen
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
    }
  },
}
</script>
