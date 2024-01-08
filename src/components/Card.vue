<template>
  <div
    class="card"
    :class="{ disabled: isDisabled }"
    :style="{
      height: `${
        (920 - 16 * Math.sqrt(settings.cardsSet.length)) /
          Math.sqrt(settings.cardsSet.length) -
        16
      }px`,
      width: `${
        (((920 - 16 * 4) / Math.sqrt(settings.cardsSet.length) - 16) * 3) / 4
      }px`,
    }"
  >
    <div
      class="card_inner"
      :class="{ 'is-flipped': isFlipped }"
      @click="onToggleFlipCard()"
    >
      <div class="card_face card_face-front">
        <div class="card-content"></div>
      </div>

      <div class="card_face card_face-back">
        <div
          class="card-content"
          :style="{
            backgroundImage: `url(${require('@/assets/' + imgBackFaceUrl)})`,
          }"
        ></div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    imgBackFaceUrl: {
      type: String,
      required: true,
    },
    card: {
      type: [String, Number, Array, Object],
    },
    settings: {
      type: Object,
      default: function () {
        return {};
      },
    },
  },
  data() {
    return {
      isDisabled: false,
      isFlipped: false,
    };
  },
  methods: {
    onToggleFlipCard() {
      if (this.isDisabled === true) {
        return false;
      }
      this.isFlipped = !this.isFlipped;
      if (this.isFlipped) {
        this.$emit("onFlip", this.card);
      } else {
        this.$emit("onClose", this.card);
      }
    },
    //onFlipBackCard
    onCloseCard() {
      this.isFlipped = false;
    },

    onAddDisabledMode() {
      this.isDisabled = true;
    },
  },
};
</script>

<style lang="css" scoped>
.card {
  display: inline-block;
  margin-right: 0.5rem;
  margin-left: 0.5rem;
  margin-top: 0.5rem;
  margin-bottom: 0.5rem;
}

.card_inner {
  width: 100%;
  height: 100%;
  transition: transform 0.5s;
  transform-style: preserve-3d;
  cursor: pointer;
  position: relative;
}

.card_inner.is-flipped {
  transform: rotateY(-180deg);
}

.card_face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  overflow: hidden;
  border-radius: 1rem;
  box-shadow: 0 3px 10px 3px rgba(0, 0, 0, 0.2);
}
.card_face-front {
  background-color: var(--light);
}
.card_face-back {
  background-color: var(--azur);
  transform: rotateY(180deg);
}
.card_face-front .card-content {
  width: 100%;
  height: 100%;
  background: url(../assets/logo.png) no-repeat center center;
  background-size: contain;
}
.card_face-back .card-content {
  width: 100%;
  height: 100%;
  background-size: contain;
  /* background-image: url("../assets/images/AcastaShipyardIcon.png"); */
}

.card.disabled .card_inner {
  cursor: default;
}
</style>
