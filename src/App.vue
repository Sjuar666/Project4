<template>
  <div class="content">
    <div>
      <h1 class="main-title">Countdown Timer</h1>
      <h2>by Samuel Juarez</h2>
    </div>

    <div class="formItems">
      <input v-model.number="timeInput" type="number" placeholder="Enter time in seconds" />
      <button @click="startTimer" type="button">Start Timer</button>
    </div>

    <div class="toDo">
      <h2>Time Remaining:</h2>
      <div id="timerDisplay">{{ formattedTime }}</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      timeInput: 0,
      timeRemaining: 0,
      timerInterval: null,
    };
  },
  computed: {
    formattedTime() {
      const minutes = Math.floor(this.timeRemaining / 60);
      const seconds = this.timeRemaining % 60;
      return `${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`;
    },
  },
  methods: {
    startTimer() {
      if (this.timeInput > 0) {
        this.timeRemaining = this.timeInput;

        if (this.timerInterval) {
          clearInterval(this.timerInterval);
        }

        this.timerInterval = setInterval(() => {
          if (this.timeRemaining > 0) {
            this.timeRemaining--;
          } else {
            clearInterval(this.timerInterval);
            alert('Time is up!');
          }
        }, 1000);
      } else {
        alert('Please enter a valid number of seconds.');
      }
    },
  },
  beforeUnmount() {
    if (this.timerInterval) {
      clearInterval(this.timerInterval);
    }
  },
};
</script>

<style scoped>
/* Your CSS */
.content {
  text-align: center;
  margin-top: 50px;
}

.main-title {
  font-size: 2.5em;
}

.formItems {
  margin: 20px 0;
}

.formItems input {
  padding: 10px;
  width: 200px;
  margin-right: 10px;
}

.formItems button {
  padding: 10px 20px;
  cursor: pointer;
}

.toDo {
  font-size: 1.5em;
}

#timerDisplay {
  font-size: 2.5em;
  margin-top: 20px;
}
</style>