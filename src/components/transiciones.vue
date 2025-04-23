<template>
  <div class="timer-container">
    <h1>{{ isPomodoro ? 'Temporizador' : 'Descanso' }}</h1>
    <p>{{ formattedTime }}</p>
    <button @click="toggleTimer">{{ isRunning ? 'Pausa' : 'Iniciar' }}</button>
    <button v-if="!isRunning" @click="resetTimer">Reiniciar</button>
  </div>
</template>

<script>
export default {
  name: 'MergedTimerWithTransitions',
  data() {
    return {
      timeLeft: 305,
      isRunning: false,
      isPomodoro: true,
      interval: null,
      backgroundColor: 'lightgreen',
    };
  },
  computed: {
    formattedTime() {
      const minutes = Math.floor(this.timeLeft / 60);
      const seconds = this.timeLeft % 60;
      return `${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`;
    }
  },
  watch: {
    timeLeft(newVal) {
      if (newVal <= 60) {
        this.backgroundColor = 'lightcoral';
      } else if (newVal <= 300) {
        this.backgroundColor = 'khaki';
      } else {
        this.backgroundColor = 'lightgreen';
      }
      document.body.style.transition = 'background-color 2s ease';
      document.body.style.backgroundColor = this.backgroundColor;
    }
  },
  methods: {
    toggleTimer() {
      if (this.isRunning) {
        clearInterval(this.interval);
      } else {
        this.interval = setInterval(() => {
          if (this.timeLeft > 0) {
            this.timeLeft--;
          } else {
            clearInterval(this.interval);
            this.isRunning = false;
          }
        }, 1000);
      }
      this.isRunning = !this.isRunning;
    },
    resetTimer() {
      clearInterval(this.interval);
      this.timeLeft = 305;
      this.isRunning = false;
    }
  },
  mounted() {
    document.body.style.transition = 'background-color 0.5s ease';
    document.body.style.backgroundColor = this.backgroundColor;
  },
  beforeDestroy() {
    clearInterval(this.interval);
    document.body.style.backgroundColor = '';
  }
};
</script>

<style scoped>
.timer-container {
  padding: 2rem;
  text-align: center;
  min-height: 200px;
  border-radius: 10px;
}
</style>
