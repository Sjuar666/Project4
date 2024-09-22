<template>
  <div class="content">
    <h1 class="main-title">Countdown Timer</h1>

    <!-- Input component to start the timer -->
    <TimerInput @start-countdown="handleStartCountdown" />

    <!-- Display component showing the current countdown -->
    <TimerDisplay :timeRemaining="timeRemaining" />

    <!-- HistoryList component showing countdown history -->
    <HistoryList :history="history" @delete-history="handleDeleteHistory" />

    <!-- UsageCounter component showing the number of times the timer has been used -->
    <UsageCounter :usageCount="usageCount" />
  </div>
</template>

<script>
import TimerInput from './components/TimerInput.vue';
import TimerDisplay from './components/TimerDisplay.vue';
import HistoryList from './components/HistoryList.vue';
import UsageCounter from './components/UsageCounter.vue';

export default {
  components: {
    TimerInput,
    TimerDisplay,
    HistoryList,
    UsageCounter
  },
  data() {
    return {
      timeRemaining: 0,  // Total time in seconds
      inputTime: 0,      // The time inputted by the user
      interval: null,    // Timer interval
      history: [],       // Array to store countdown history
      usageCount: 0      // Track how many times the timer has been used
    };
  },
  created() {
    // Load history and usage count from localStorage when the component is created
    this.loadFromLocalStorage();
  },
  methods: {
    handleStartCountdown(time) {
      this.inputTime = time; // Store the input time
      this.timeRemaining = time;
      this.startCountdown();
      this.incrementUsageCount(); // Increment the usage count when countdown starts
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

      // Save history to localStorage
      this.saveToLocalStorage();
    },
    handleDeleteHistory(index) {
      // Remove the selected history entry
      this.history.splice(index, 1);

      // Save history to localStorage after deletion
      this.saveToLocalStorage();
    },
    incrementUsageCount() {
      this.usageCount += 1;

      // Save the updated usage count to localStorage
      this.saveToLocalStorage();
    },
    saveToLocalStorage() {
      // Save history and usage count to localStorage
      localStorage.setItem('countdownHistory', JSON.stringify(this.history));
      localStorage.setItem('usageCount', this.usageCount);
    },
    loadFromLocalStorage() {
      // Load history and usage count from localStorage if available
      const savedHistory = localStorage.getItem('countdownHistory');
      const savedUsageCount = localStorage.getItem('usageCount');

      if (savedHistory) {
        this.history = JSON.parse(savedHistory);
      }

      if (savedUsageCount) {
        this.usageCount = parseInt(savedUsageCount, 10);
      }
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

<style scoped>
/* Add styles to make the countdown timer and history look good */
.content {
  font-family: Arial, Helvetica, sans-serif;
  height: 100vh;
  background: #f0f0f0;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}

.main-title {
  font-size: 3em;
  color: teal;
}

.history {
  margin-top: 40px;
}
</style>


