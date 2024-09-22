<template>
    <div id="timerDisplay">
      <h2>{{ formattedTime }}</h2>
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
  