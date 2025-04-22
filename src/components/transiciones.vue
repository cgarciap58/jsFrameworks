<template>
    <div id="app">
        <h1>{{ isPomodoro ? 'Temporizador' : 'Descanso' }}</h1>
        <p>{{ formattedTime }}</p>
        <button @click="toggleTimer">{{ isRunning ? 'Pausa' : 'Iniciar' }}</button>
        <button v-if="!isRunning" @click="resetTimer">Reiniciar</button>
        <button @click="changeBackgroundColor">Change Background</button>
    </div>
  </template>
  
  <script>
  export default {
    name: 'Temporizador',
    data() {
      return {
        // Initial background color
        backgroundColor: 'lightblue',
        timeLeft: 305,
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
    // Check the timeLeft and return the appropriate color
    if (this.timeLeft <= 60) return 'lightcoral';
    else if (this.timeLeft <= 300) return 'yellow';
    else return 'red';
  }
},
    methods: {
      changeBackgroundColor() {
        // Toggle between two background colors on button click
        this.backgroundColor = this.backgroundColor === 'lightblue' ? 'lightcoral' : 'lightblue';
      },
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
  this.timeLeft = this.isPomodoro ? 1500 : 300; // Corrected timer reset logic
},
    switchToBreak() {
      this.isPomodoro = !this.isPomodoro;
      this.timeLeft = this.isPomodoro ? 1500 : 300;
    }
    },
    watch: {
      // Watch for changes in the background color and update the body style
      backgroundColor(newColor) {
        document.body.style.backgroundColor = newColor;
      },
    },
    mounted() {
      // Initialize the body background color on mount
      document.body.style.transition = 'background-color 3s ease'; // Smooth transition effect
      document.body.style.backgroundColor = this.backgroundColor; // Set the initial background
    }
  };
  </script>
  
  <style scoped>
  /* You can add other styles here */
  button {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
  }
  </style>
  