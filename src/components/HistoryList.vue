<template>
    <div class="history">
      <h3 v-if="history.length > 0">Countdown History</h3>
      <ul>
        <li v-for="(item, index) in history" :key="index">
          {{ item }}
          <button @click="deleteHistory(index)">Delete</button>
        </li>
      </ul>
    </div>
  </template>
  
  <script>
  export default {
    props: {
      history: {
        type: Array,
        required: true
      }
    },
    methods: {
      deleteHistory(index) {
        this.$emit('delete-history', index);
      }
    }
  };
  </script>
  
  
  