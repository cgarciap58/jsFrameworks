<template>
  <div class="temporizador">
    <h1>{{ isPomodoro ? 'Temporizador' : 'Descanso' }}</h1>
    <p>{{ formattedTime }}</p>
    <button @click="toggleTimer">{{ isRunning ? 'Pausa' : 'Iniciar' }}</button>
    <button v-if="!isRunning" @click="resetTimer">Reiniciar</button>
  </div>
</template>

<script>
export default {
  name: 'Temporizador',
  data() {
    return {
      timeLeft: 65,
      isRunning: false,
      isPomodoro: true,
      interval: null,
    };
  },
  computed: {
    formattedTime() {
      const minutes = Math.floor(this.timeLeft / 60);
      const seconds = this.timeLeft % 60;
      return `${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`;
    },
    backgroundClass() {
      if (this.timeLeft <= 60) return 'bg-red';
      else if (this.timeLeft <= 300) return 'bg-yellow';
      else if (this.timeLeft <= 600) return 'bg-green';
      else return 'bg-green';
    }
  },
  watch: {
    backgroundClass(newClass, oldClass) {
      if (oldClass) {
        document.body.classList.remove(oldClass);
      }
      document.body.classList.add(newClass);
    }
  },
  mounted() {
    // Initial background setup
    document.body.classList.add(this.backgroundClass);
  },
  beforeUnmount() {
    clearInterval(this.interval);
    // Remove any background class when component is destroyed
    document.body.classList.remove(this.backgroundClass);
  },
  methods: {
    toggleTimer() {
      this.isRunning ? this.pauseTimer() : this.startTimer();
    },
    startTimer() {
      this.isRunning = true;
      this.interval = setInterval(() => {
        if (this.timeLeft > 0) {
          this.timeLeft -= 1;
        } else {
          this.switchToBreak();
        }
      }, 1000);
    },
    pauseTimer() {
      clearInterval(this.interval);
      this.isRunning = false;
    },
    resetTimer() {
      clearInterval(this.interval);
      this.isRunning = false;
      this.timeLeft = this.isPomodoro ? 305 : 300;
    },
    switchToBreak() {
      this.isPomodoro = !this.isPomodoro;
      this.timeLeft = this.isPomodoro ? 1500 : 300;
    }
  }
};
</script>

<style scoped>
.temporizador {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 20px;
  background-color: #ffffffcc;
  border-radius: 8px;
  width: 300px;
  margin: 50px auto;
  font-family: Arial, sans-serif;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
}

button {
  padding: 10px 20px;
  margin: 10px;
  font-size: 16px;
  cursor: pointer;
  border: none;
  border-radius: 4px;
  transition: background 0.3s ease;
}

button:hover {
  background-color: #4CAF50;
  color: white;
}

h1 {
  font-size: 24px;
  margin-bottom: 20px;
}

p {
  font-size: 48px;
  font-weight: bold;
}
</style>
