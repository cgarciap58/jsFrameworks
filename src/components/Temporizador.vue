<template>
  <div class="timer-box">
    <h1 class="title">{{ isPomodoro ? 'Temporizador' : 'Descanso' }}</h1>
    <div class="time">{{ formattedTime }}</div>
    <div class="button-group">
      <button class="main-btn" @click="toggleTimer">{{ isRunning ? 'Pausa' : 'Iniciar' }}</button>
      <button v-if="!isRunning" class="reset-btn" @click="resetTimer">Reiniciar</button>
    </div>
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
        this.backgroundColor = 'red';
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
    document.body.style.transition = 'background-color 2s ease';
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

.timer-box {
  max-width: 400px;
  min-width: 350px;
  margin: 100px auto;
  background-color: rgb(12, 12, 12, 0.75);
  border-radius: 20px;
  padding: 2rem;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  text-align: center;
}

.title {
  font-size: 2rem;
  margin-bottom: 1rem;
  color: rgb(227, 212, 212);
}

.time {
  font-size: 5rem;
  font-weight: bold;
  margin: 1.5rem 0;
  color: rgb(225, 209, 209);
}

.button-group {
  display: flex;
  justify-content: center;
  gap: 1rem;
}

.main-btn,
.reset-btn {
  font-size: 1.2rem;
  padding: 1rem 2rem;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  transition: all 0.2s ease;
}

.main-btn {
  background-color: #ff4b4b;
  color: white;
}

.main-btn:hover {
  background-color: #e63a3a;
}

.reset-btn {
  background-color: #eeeeee;
  color: #333;
}

.reset-btn:hover {
  background-color: #dddddd;
}


</style>
