<template>
  <div class="screen">
    <div
      class="screen-inner"
      :style="{
        width: `${
          ((((920 - 16 * 4) / Math.sqrt(settings.cardsSet.length) - 16) * 3) /
            4 +
            16) *
          Math.sqrt(settings.cardsSet.length)
        }px`,
        zoom: `${
          settings.cardsSet.length === 16 || settings.cardsSet.length === 36
            ? '0.8'
            : '0.9'
        }`,
      }"
    >
      <card
        v-for="(card, index) in settings.cardsSet"
        :key="index"
        :ref="`card-${index}`"
        :imgBackFaceUrl="`images/${randomImage[settings.cardsSet[index] - 1]}`"
        :card="{ index: index, value: card }"
        :settings="settings"
        @onFlip="checkRule($event)"
      ></card>
    </div>
  </div>
</template>

<script>
import Card from "./Card.vue";
import { allFiles } from "../utils/filesIterator";
export default {
  data() {
    return {
      randomImage: this.getRandomImage(),
      rules: [],
    };
  },
  props: {
    settings: {
      type: Object,
      default: function () {
        return {};
      },
    },
  },
  components: {
    Card,
  },
  methods: {
    getRandomImage() {
      const rands = [];
      const result = [];
      while (rands.length < this.settings.cardsSet.length / 2) {
        const r = Math.floor(Math.random() * allFiles.length);
        if (rands.indexOf(r) === -1) {
          rands.push(r);
        }
      }
      for (let i = 0; i < rands.length; i++) {
        result.push(allFiles[rands[i]]);
      }
      return result;
    },
    checkRule(card) {
      if (this.rules.length === 2) return false;
      this.rules.push(card);
      // If choose matched cards
      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value
      ) {
        // add disabled class
        this.$refs[`card-${this.rules[0].index}`][0].onAddDisabledMode();
        this.$refs[`card-${this.rules[1].index}`][0].onAddDisabledMode();

        //reset rules
        this.rules = [];

        //Win condition
        const disabledEmlements = document.querySelectorAll(
          ".screen .card.disabled"
        );
        setTimeout(() => {
          if (disabledEmlements.length === this.settings.cardsSet.length - 2) {
            this.$emit("onFinish");
          }
        }, 800);
      }
      // If choose unmatched cards
      else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value
      ) {
        let ruleIndex0 = this.rules[0];
        let ruleIndex1 = this.rules[1];
        //Reset array rules
        this.rules = [];
        setTimeout(() => {
          //Close two cards
          this.$refs[`card-${ruleIndex0.index}`][0].onCloseCard();
          this.$refs[`card-${ruleIndex1.index}`][0].onCloseCard();
        }, 800);
      } else return false;
    },
  },
};
</script>

<style lang="css" scoped>
.screen {
  width: 100%;
  height: 100vh;
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  background-color: var(--dark);
  color: var(--light);
}

.screen-inner {
  display: flex;
  flex-wrap: wrap;
  margin: 1rem auto;
  -moz-transform: scale(3);
  -moz-transform-origin: 0 0;
}
</style>
