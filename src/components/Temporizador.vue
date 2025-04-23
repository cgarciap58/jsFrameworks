<template>
  <!-- Contenedor principal del temporizador -->
  <div class="timer-box">
    <!-- Título que cambia según el estado del temporizador -->
    <h1 class="title">{{ isPomodoro ? 'Temporizador' : 'Descanso' }}</h1>
    
    <!-- Tiempo formateado mostrado en pantalla -->
    <div class="time">{{ formattedTime }}</div>
    
    <!-- Botones de control: Iniciar/Pausar y Reiniciar -->
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
      // Tiempo restante en segundos (inicial: 5 min y 5 seg)
      timeLeft: 305,
      // Estado de ejecución del temporizador
      isRunning: false,
      // Indica si es un bloque de trabajo o descanso
      isPomodoro: true,
      // Referencia al intervalo del temporizador
      interval: null,
      // Color de fondo actual
      backgroundColor: 'lightgreen',
    };
  },
  computed: {
    // Formatea el tiempo restante en formato MM:SS
    formattedTime() {
      const minutes = Math.floor(this.timeLeft / 60);
      const seconds = this.timeLeft % 60;
      return `${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`;
    }
  },
  watch: {
    // Observa cambios en el tiempo restante
    timeLeft(newVal) {
      // Cambia el color de fondo según el tiempo restante
      if (newVal <= 60) {
        this.backgroundColor = 'red';       // Último minuto
      } else if (newVal <= 300) {
        this.backgroundColor = 'khaki';     // Últimos 5 minutos
      } else {
        this.backgroundColor = 'lightgreen';// Más de 5 minutos
      }
      // Aplica transición suave al cambio de color
      document.body.style.transition = 'background-color 2s ease';
      document.body.style.backgroundColor = this.backgroundColor;
    }
  },
  methods: {
    // Inicia o pausa el temporizador
    toggleTimer() {
      if (this.isRunning) {
        // Si está corriendo, detiene el intervalo
        clearInterval(this.interval);
      } else {
        // Si no está corriendo, inicia el intervalo cada segundo
        this.interval = setInterval(() => {
          if (this.timeLeft > 0) {
            this.timeLeft--;
          } else {
            // Al llegar a 0, detiene el temporizador
            clearInterval(this.interval);
            this.isRunning = false;
          }
        }, 1000);
      }
      // Alterna el estado de ejecución
      this.isRunning = !this.isRunning;
    },
    // Reinicia el temporizador a su estado inicial
    resetTimer() {
      clearInterval(this.interval);
      this.timeLeft = 305;
      this.isRunning = false;
    }
  },
  mounted() {
    // Establece el color de fondo inicial al montar el componente
    document.body.style.transition = 'background-color 2s ease';
    document.body.style.backgroundColor = this.backgroundColor;
  },
  beforeDestroy() {
    // Limpia el intervalo y restaura el fondo al destruir el componente
    clearInterval(this.interval);
    document.body.style.backgroundColor = '';
  }
};
</script>

<style scoped>
/* Estilos del contenedor */
.timer-container {
  padding: 2rem;
  text-align: center;
  min-height: 200px;
  border-radius: 10px;
}

/* Caja visual del temporizador */
.timer-box {
  max-width: 400px;
  min-width: 350px;
  margin: 10px auto;
  background-color: rgb(12, 12, 12, 0.75);
  border-radius: 20px;
  padding: 2rem;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
  text-align: center;
}

/* Título del temporizador */
.title {
  font-size: 2rem;
  margin-bottom: 1rem;
  color: rgb(227, 212, 212);
}

/* Visualización del tiempo */
.time {
  font-size: 5rem;
  font-weight: bold;
  margin: 1.5rem 0;
  color: rgb(225, 209, 209);
}

/* Grupo de botones */
.button-group {
  display: flex;
  justify-content: center;
  gap: 1rem;
}

/* Botones de acción */
.main-btn,
.reset-btn {
  font-size: 1.2rem;
  padding: 1rem 2rem;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  transition: all 0.2s ease;
}

/* Botón principal (Iniciar/Pausar) */
.main-btn {
  background-color: #ff4b4b;
  color: white;
}
.main-btn:hover {
  background-color: #e63a3a;
}

/* Botón de reinicio */
.reset-btn {
  background-color: #eeeeee;
  color: #333;
}
.reset-btn:hover {
  background-color: #dddddd;
}
</style>
