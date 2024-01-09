<template>
  <div class="screen">
    <div class="back-btn" @click="onBack()">Back</div>
    <div
      class="screen-inner"
      :style="{
        width: `${
          ((((920 - 16 * 4) / Math.sqrt(this.settings.cardsSet.length) - 16) *
            3) /
            4 +
            16) *
          Math.sqrt(this.settings.cardsSet.length)
        }px`,
        zoom: `${
          settings.cardsSet.length === 16 || settings.cardsSet.length === 36
            ? '0.8'
            : '0.8'
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
        @click="console.log($event)"
        @onFlip="checkRule($event)"
        @onClose="resetRule($event)"
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
      isNotClosing: true, //check neu card van dang closing de tranh khi back ma chua close xong se gay loi
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
      // If choose matched cards -----------------------------------------------------------------------------
      if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value &&
        this.rules[0].index !== this.rules[1].index
      ) {
        console.log("Correct ...");
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
      // If choose unmatched cards ----------------------------------------------------------------------------
      else if (
        this.rules.length === 2 &&
        this.rules[0].value !== this.rules[1].value &&
        this.rules[0].index !== this.rules[1].index
      ) {
        let ruleIndex0 = this.rules[0];
        let ruleIndex1 = this.rules[1];
        //Reset array rules
        this.rules = [];
        this.isNotClosing = false; //Card dang lat up xuong
        setTimeout(() => {
          //Close two cards
          this.$refs[`card-${ruleIndex0.index}`][0].onCloseCard();
          this.$refs[`card-${ruleIndex1.index}`][0].onCloseCard();
          console.log("Wrong ...");
          this.isNotClosing = true; //Card da lat up xuong xong
        }, 800);
      }
      // If choose same card twice really fast --------------------------------------------------------------------
      else if (
        this.rules.length === 2 &&
        this.rules[0].value === this.rules[1].value &&
        this.rules[0].index === this.rules[1].index
      ) {
        console.log("Bug ...");
        this.rules = [];
        this.rules.push(card);
      } else return false;
    },
    resetRule() {
      this.rules = [];
    },
    onBack() {
      if (this.isNotClosing) {
        this.$emit("onBackMainScreen");
      }
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

.back-btn {
  position: absolute;
  top: 1.5rem;
  left: 2rem;
  display: inline-block;
  background: transparent;
  box-shadow: none;
  padding: 1rem 2rem;
  border: 1px solid var(--light);
  border-radius: 1rem;
}

.back-btn:hover {
  background-color: var(--light);
  color: var(--dark);
  cursor: pointer;
}

/* Responsive cho may nho */
@media screen and (max-width: 480px) and (min-width: 258px) {
  .screen-inner {
    padding-top: 10rem;
    zoom: 0.5 !important;
  }
}

@media screen and (max-width: 1024px) and (min-width: 480px) {
  .screen-inner {
    padding-top: 1rem;
    zoom: 0.6 !important;
  }
}

@media screen and (max-width: 1024px) and (min-width: 480px) and (orientation: landscape) {
  .screen {
    height: auto;
  }
}

@media screen and (min-width: 1024px) and (orientation: landscape) {
  .screen {
    height: 100vh;
  }
}
</style>
