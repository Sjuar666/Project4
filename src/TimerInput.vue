<template>
    <div class="timer-input">
      <input v-model.number="time" type="number" placeholder="Enter time in seconds" />
      <button @click="startTimer">Start Timer</button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        time: 0 // Input value for time
      };
    },
    methods: {
      startTimer() {
        if (this.time > 0) {
          this.$emit('start-countdown', this.time); // Emit event to the parent
        }
      }
    }
  };
  </script>
  
  <style scoped>
  .timer-input {
    margin-top: 20px;
  }
  </style>
  