<template>
  <div class="flex justify-center items-center h-screen">
    <div class="flex w-full ml-12 mr-12">
      <div v-for="(column, index) in columns" :key="index" class="flex-1 mr-2">
        <h2 class="text-xl font-semibold mb-4">{{ column.title }} ({{ column.tasks.length }})</h2>
        <div class="border border-gray-400 pl-12 rounded">
          <!-- Ensure draggable is only used in the client-side -->
          <draggable v-if="isClient" v-model="column.tasks" :options="{ moves: (el, container, handle) => handle.classList.contains('handle') }">
            <div class="grid grid-cols-1 gap-4">
              <TaskCard
                v-for="(task, idx) in column.tasks"
                :key="idx"
                :task="task"
                @edit-task="editTask(task)"
                @delete-task="deleteTask(task)"
              />
            </div>
          </draggable>
          <button class="mt-2 font-bold py-2 px-4 rounded" @click="addTask(column.title)">+ New</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import TaskCard from '~/components/TaskCard.vue';

export default {
  components: {
    TaskCard,
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
  computed: {
    // Check if the code is running in the client-side
    isClient() {
      return process.client;
    }
  },
  methods: {
    addTask(status) {
      const title = prompt('Enter task title:');
      if (title) {
        this.columns.find(column => column.title === status).tasks.push({ title });
      }
    },
    editTask(task) {
      const newTitle = prompt('Enter new task title:', task.title);
      if (newTitle !== null) {
        task.title = newTitle;
      }
    },
    deleteTask(task) {
      if (confirm('Are you sure you want to delete this task?')) {
        // Loop through all columns to find and delete the task
        for (const column of this.columns) {
          const index = column.tasks.indexOf(task);
          if (index !== -1) {
            column.tasks.splice(index, 1);
            break; // Exit loop once task is deleted
          }
        }
      }
    }
  }
};
</script>
