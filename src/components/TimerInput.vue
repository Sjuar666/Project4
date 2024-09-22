<template>
    <div class="formItems">
      <input v-model.number="time" type="number" placeholder="Enter time in seconds" />
      <button @click="startTimer">Start Timer</button>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        time: 0
      };
    },
    methods: {
      startTimer() {
        if (this.time > 0) {
          this.$emit('start-countdown', this.time);
        }
      }
    }
  };
  </script>
  