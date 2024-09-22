<template>
    <div class="usage-counter">
      <h3>Countdown Usage</h3>
      <p>This timer has been used {{ usageCount }} times.</p>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      usageCount: {
        type: Number,
        required: true
      }
    }
  };
  </script>
  
