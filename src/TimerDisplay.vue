<template>
    <div class="timer-display">
      <h2>Time Remaining: {{ formattedTime }}</h2>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      timeRemaining: {
        type: Number,
        required: true
      }
    },
    computed: {
      formattedTime() {
        const minutes = Math.floor(this.timeRemaining / 60);
        const seconds = this.timeRemaining % 60;
        return `${String(minutes).padStart(2, '0')}:${String(seconds).padStart(2, '0')}`;
      }
    }
  };
  </script>
  
  <style scoped>
  .timer-display {
    font-size: 2rem;
    margin-top: 20px;
  }
  </style>
  