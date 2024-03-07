<template>
  <div>
    <!-- Drag and drop tasks -->
    <draggable v-model="columns" @end="onDragEnd">
      <div v-for="(column, index) in columns" :key="index" class="column">
        <h2>{{ column.title }} ({{ column.tasks.length }})</h2>
        <div class="tasks">
          <div v-for="(task, taskIndex) in column.tasks" :key="taskIndex" class="task" @click="openTaskDetails(task)">
            {{ task.title }}
          </div>
          <button @click="addTask(index)">+ New</button>
        </div>
      </div>
    </draggable>
  </div>
</template>

<script>
import draggable from 'vue-draggable';

export default {
  components: {
    draggable,
  },
  data() {
    return {
      columns: [
        { title: 'To Do', tasks: [] },
        { title: 'In Progress', tasks: [] },
        { title: 'Completed', tasks: [] }
      ]
    };
  },
  methods: {
    addTask(index) {
      const title = prompt('Enter task title:');
      if (title) {
        this.columns[index].tasks.push({ title });
      }
    },
    openTaskDetails(task) {
      // Implement logic to open task details page/modal
    },
    onDragEnd(event) {
      // Implement logic to handle task movement after drag and drop
    }
  }
};
</script>

<style>
/* Add your CSS styles here */
.column {
  /* Style for columns */
}
.tasks {
  /* Style for tasks container */
}
.task {
  /* Style for individual tasks */
}
</style>
