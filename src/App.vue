<template>
  <div id="app">
    <h1>Countdown Timer</h1>
    <TimerInput @start-countdown="handleStartCountdown" />
    <TimerDisplay :timeRemaining="timeRemaining" />
  </div>
</template>

<script>
import TimerDisplay from './TimerDisplay.vue';
import TimerInput from './TimerInput.vue';

export default {
  components: {
    TimerInput,
    TimerDisplay
  },
  data() {
    return {
      timeRemaining: 0,  // Total time in seconds
      interval: null
    };
  },
  methods: {
    handleStartCountdown(time) {
      this.timeRemaining = time;
      this.startCountdown();
    },
    startCountdown() {
      if (this.interval) {
        clearInterval(this.interval);
      }

      this.interval = setInterval(() => {
        if (this.timeRemaining > 0) {
          this.timeRemaining -= 1;
        } else {
          clearInterval(this.interval);
        }
      }, 1000);
    }
  },
  beforeDestroy() {
    if (this.interval) {
      clearInterval(this.interval);
    }
  }
};
</script>

<style scoped>
#app {
  text-align: center;
  margin-top: 40px;
}
</style>
