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
        // Emit an event to parent component to delete this specific item
        this.$emit('delete-history', index);
      }
    }
  };
  </script>
  
  <style scoped>
  ul {
    list-style: none;
    padding: 0;
  }
  
  li {
    background-color: #e0f7fa;
    padding: 10px;
    margin-bottom: 10px;
    border-left: 5px solid teal;
    border-radius: 4px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  
  button {
    background-color: red;
    color: white;
    border: none;
    padding: 5px 10px;
    border-radius: 4px;
    cursor: pointer;
  }
  
  button:hover {
    background-color: darkred;
  }
  </style>
  