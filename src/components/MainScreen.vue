<template>
  <div class="screen">
    <h1>AZUR LANE FLIPPER</h1>
    <span>AZVN❤ #9kmember</span>
    <p>Select mode to start game</p>
    <div class="modes">
      <button @click="onStart(16, screenHeight)">
        <img src="../assets/gif/easy.gif" alt="" />
        <p>4x4</p>
        <span>Easy</span>
      </button>

      <button @click="onStart(36, screenHeight)">
        <img src="../assets/gif/normal.gif" alt="" />
        <p>6x6</p>
        <span>Normal</span>
      </button>

      <button @click="onStart(64, screenHeight)">
        <img src="../assets/gif/hard.gif" alt="" />
        <p>8x8</p>
        <span>Hard</span>
      </button>

      <button @click="onStart(100, screenHeight)">
        <img src="../assets/gif/superhard.gif" alt="" />
        <p>10x10</p>
        <span>Very hard</span>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      screenHeight: screen.height,
    };
  },
  methods: {
    onStart(totalBlock, screenHeight) {
      this.$emit("onStart", { totalBlock, screenHeight });
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
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  background-color: var(--dark);
  color: var(--light);
}

.screen h1 {
  font-size: 4.5rem;
  text-transform: uppercase;
}

.screen p {
  font-size: 2rem;
}

.modes {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: center;
  margin-top: 2rem;
}

.modes button {
  font: var(--font);
  width: 150px;
  height: 150px;
  background: transparent;
  box-shadow: none;
  border: 1px solid var(--light);
  color: var(--light);
  display: flex;
  flex-direction: column;
  border-radius: 1rem;
  margin: 1rem;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: background 0.3s ease-in-out;
}

.modes button img {
  max-width: 45%;
}

.modes button:hover {
  background-color: var(--light);
  color: var(--dark);
}

.modes button span:first-child {
  font-size: 2rem;
}

.modes button span:last-child {
  display: block;
  font-size: 1.25rem;
  margin-top: 0.5rem;
}
/* Responsive cho may nho */
@media screen and (max-width: 259px) {
  .screen {
    height: max-content;
  }
}

@media screen and (max-width: 480px) and (min-width: 258px) {
  .modes button {
    width: 30%;
    height: auto;
  }
  .screen h1 {
    font-size: 4.5rem;
    text-transform: uppercase;
    text-align: center;
  }

  .screen {
    padding-top: 2rem;
    justify-content: start;
  }
}

@media screen and (max-width: 1024px) and (min-width: 480px) {
  .screen {
    justify-content: start;
    padding-top: 10rem;
  }
  .modes button {
    width: 20%;
    height: auto;
  }
}

@media screen and (max-width: 1024px) and (min-width: 258px) and (orientation: landscape) {
  .screen {
    height: auto;
  }
}

@media screen and (min-width: 1024px) {
  .modes button {
    padding: 12px;
    width: 10%;
    height: auto;
  }
}

@media screen and (min-width: 1024px) and (orientation: landscape) {
  .screen {
    height: 100vh;
  }
}
</style>
