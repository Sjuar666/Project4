<template>
  <div class="content">
    <h1 class="main-title">Countdown Timer</h1>

    <TimerInput @start-countdown="handleStartCountdown" />
    <TimerDisplay :timeRemaining="timeRemaining" />
    <HistoryList :history="history" @delete-history="handleDeleteHistory" />
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
      timeRemaining: 0,  
      inputTime: 0,      
      interval: null,    
      history: [],       
      usageCount: 0      
    };
  },
  created() {
    this.loadFromLocalStorage();
  },
  methods: {
    handleStartCountdown(time) {
      this.inputTime = time; 
      this.timeRemaining = time;
      this.startCountdown();
      this.incrementUsageCount(); 
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
          this.addToHistory(); 
        }
      }, 1000);
    },
    addToHistory() {
      const date = new Date();
      const formattedTime = date.toLocaleTimeString();
      const formattedInputTime = new Date(this.inputTime * 1000).toISOString().substr(11, 8); // Format input time to HH:mm:ss
      this.history.unshift(
        `Countdown of ${formattedInputTime} finished at ${formattedTime}`
      );

      this.saveToLocalStorage();
    },
    handleDeleteHistory(index) {
      this.history.splice(index, 1);
      this.saveToLocalStorage();
    },
    incrementUsageCount() {
      this.usageCount += 1;
      this.saveToLocalStorage();
    },
    saveToLocalStorage() {
      localStorage.setItem('countdownHistory', JSON.stringify(this.history));
      localStorage.setItem('usageCount', this.usageCount);
    },
    loadFromLocalStorage() {
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
    if (this.interval) {
      clearInterval(this.interval);
    }
  }
};
</script>



