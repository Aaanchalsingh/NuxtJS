<!-- components/TaskCard.vue -->
<template>
    <div class="bg-white p-4 mb-2 rounded shadow">
      <div class="flex justify-between">
        <h3>{{ task.title }}</h3>
        <button class="handle">☰</button>
      </div>
      <!-- Add more task details here -->
    </div>
  </template>

  <script>
  export default {
    props: {
      task: {
        type: Object,
        required: true
      }
    }
  };
  </script>
