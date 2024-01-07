<!--  -->
<!--  -->
<!--  -->
<template>
  <main-screen
    v-if="statusMatch === 'default'"
    @onStart="changeStatus($event, 'interact')"
  >
  </main-screen>
  <interact-screen
    v-if="statusMatch === 'interact'"
    v-bind:settings="settings"
    @onFinish="onGetRusult"
  >
  </interact-screen>
  <result-screen
    v-if="statusMatch === 'result'"
    :timer="timer"
    @onStartAgain="statusMatch = 'default'"
  ></result-screen>
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import { shuffle } from "./utils/array";

export default {
  name: "App",
  data() {
    return {
      settings: {
        totalBlock: 0,
        cardsSet: [],
        timeStart: null,
      },
      statusMatch: "default",
      timer: 0,
    };
  },
  components: {
    MainScreen,
    InteractScreen,
    ResultScreen,
  },
  methods: {
    changeStatus(config, status) {
      this.settings.totalBlock = config.totalBlock;

      //create two cards sets and combine them
      const firstCardsSet = Array.from(
        { length: this.settings.totalBlock / 2 },
        (_, i) => i + 1
      );
      const secondCardsSet = [...firstCardsSet];
      const combinedCardsSet = [...firstCardsSet, ...secondCardsSet];

      //assign cardsSet with combinedCardsSet after shuffle 4 times
      this.settings.cardsSet = shuffle(
        shuffle(shuffle(shuffle(combinedCardsSet)))
      );

      this.settings.timeStart = new Date().getTime();
      //data ready before changing screen
      this.statusMatch = status;
    },

    onGetRusult() {
      //Get timer
      this.timer = new Date().getTime() - this.settings.timeStart;
      //Switch to result component
      this.statusMatch = "result";
    },
  },
};
</script>
