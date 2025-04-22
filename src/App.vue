<script setup>
import Temporizador from './components/Temporizador.vue';
</script>

<template>
  <div>
    <a href="https://vite.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a>
  </div>
  <div id="app">
    <Temporizador />
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}

</style>

<style>
body {
  transition: background 1s ease-in-out;
}

.bg-default {
  background: linear-gradient(to bottom right, #e0f7fa, #ffffff);
}

.bg-green {
  background: linear-gradient(to bottom right, #81c784, #c8e6c9);
}

.bg-yellow {
  background: linear-gradient(to bottom right, #fff176, #fff9c4);
}

.bg-red {
  background: linear-gradient(to bottom right, #ff4d4d, #ffcdd2);
}
</style>
