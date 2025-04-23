<script setup>
import Temporizador from './components/Temporizador.vue';
// Aquí importamos el componente desde './components/Temporizador.vue';

</script>

<template>
  <div>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/cctimers.svg" class="logo" alt="CCTimers logo" />
    </a>
  </div>
  <div id="app">
    <Temporizador />
  </div>
</template>

<style scoped>
.logo {
  height: 10em;
  padding: 0.1em;
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
