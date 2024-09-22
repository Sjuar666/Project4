<template>
  <div class="content">
    <h1 class="main-title">Countdown Timer</h1>

    <!-- Input component to start the timer -->
    <TimerInput @start-countdown="handleStartCountdown" />

    <!-- Display component showing the current countdown -->
    <TimerDisplay :timeRemaining="timeRemaining" />

    <!-- HistoryList component showing countdown history -->
    <HistoryList :history="history" @delete-history="handleDeleteHistory" />
  </div>
</template>

<script>
import TimerInput from './components/TimerInput.vue';
import TimerDisplay from './components/TimerDisplay.vue';
import HistoryList from './components/HistoryList.vue';

export default {
  components: {
    TimerInput,
    TimerDisplay,
    HistoryList
  },
  data() {
    return {
      timeRemaining: 0,  // Total time in seconds
      inputTime: 0,      // The time inputted by the user
      interval: null,    // Timer interval
      history: []        // Array to store countdown history
    };
  },
  methods: {
    handleStartCountdown(time) {
      this.inputTime = time; // Store the input time
      this.timeRemaining = time;
      this.startCountdown();
    },
    startCountdown() {
      // Clear any existing interval
      if (this.interval) {
        clearInterval(this.interval);
      }

      // Start the countdown
      this.interval = setInterval(() => {
        if (this.timeRemaining > 0) {
          this.timeRemaining -= 1;
        } else {
          clearInterval(this.interval);
          this.addToHistory(); // Add entry to history once countdown finishes
        }
      }, 1000);
    },
    addToHistory() {
      const date = new Date();
      const formattedTime = date.toLocaleTimeString();
      const formattedInputTime = new Date(this.inputTime * 1000).toISOString().substr(11, 8); // Format input time to HH:mm:ss

      // Add both input time and the finish time to the history
      this.history.unshift(
        `Countdown of ${formattedInputTime} finished at ${formattedTime}`
      );
    },
    handleDeleteHistory(index) {
      // Remove the selected history entry
      this.history.splice(index, 1);
    }
  },
  beforeDestroy() {
    // Clear interval when the component is destroyed
    if (this.interval) {
      clearInterval(this.interval);
    }
  }
};
</script>

