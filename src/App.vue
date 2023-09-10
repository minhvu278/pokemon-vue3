<template>
  <main-screen v-if="statusMatch === 'default'" @onStart="onHandleBeforeStart($event)" />
  <interact-screen v-if="statusMatch === 'match'" />
</template>

<script>
import MainScreen from "@/components/MainScreen";
import InteractScreen from "@/components/InteractScreen";

export default {
  name: 'App',
  data() {
    return {
      statusMatch: "default"
    }
  },
  components: {
    MainScreen,
    InteractScreen
  },
  methods: {
    onHandleBeforeStart(config) {
      console.log("Ok", config)
      this.statusMatch = "match"
    }
  },
}
</script>
