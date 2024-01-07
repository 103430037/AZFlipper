<template>
  <div class="screen">
    <h1>CARDS FLIPPER</h1>
    <span>Azur Lane edition</span>
    <p>Select mode to start game</p>
    <div class="modes">
      <button @click="onStart(16)">
        <p>4x4</p>
        <span>Easy</span>
      </button>

      <button @click="onStart(36)">
        <p>6x6</p>
        <span>Normal</span>
      </button>

      <button @click="onStart(64)">
        <p>8x8</p>
        <span>Hard</span>
      </button>

      <button @click="onStart(100)">
        <p>10x10</p>
        <span>Very hard</span>
      </button>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    onStart(totalBlock) {
      this.$emit("onStart", { totalBlock });
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
  margin: 0 1rem;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: background 0.3s ease-in-out;
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
</style>
