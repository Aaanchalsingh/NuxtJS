<template>
  <div class="flex">
    <div v-for="(column, index) in columns" :key="index" class="flex-1 mr-4">
      <h2>{{ column.title }}</h2>
      <div class="mt-4">
        <!-- Ensure draggable is only used in the client-side -->
        <draggable
          v-if="isClient"
          v-model="column.tasks"
          :options="dragOptions"
          class="drag-container"
        >
          <task-card
            v-for="(task, idx) in column.tasks"
            :key="idx"
            :task="task"
            @edit="editTask(task)"
            @delete="deleteTask(task)"
          />
        </draggable>
        <button
          class="mt-2 bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded"
          @click="addTask(column.title)"
        >
          + New
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import TaskCard from "~/components/TaskCard.vue";
import draggable from 'vue-dragula';

export default {
  components: {
    TaskCard,
  },
  data() {
    return {
      columns: [
        { title: "To Do", tasks: [] },
        { title: "In Progress", tasks: [] },
        { title: "Completed", tasks: [] },
      ],
      dragOptions: {
        moves: (el, container, handle) => handle.classList.contains("handle"),
      },
    };
  },
  computed: {
    isClient() {
      return process.client;
    },
  },
  methods: {
    addTask(status) {
      const title = prompt("Enter task title:");
      if (title) {
        this.columns
          .find((column) => column.title === status)
          .tasks.push({ title });
      }
    },
    editTask(task) {
      const newTitle = prompt("Enter new task title:", task.title);
      if (newTitle !== null) {
        task.title = newTitle;
      }
    },
    deleteTask(task) {
      if (confirm("Are you sure you want to delete this task?")) {
        const columnIndex = this.columns.findIndex((column) =>
          column.tasks.includes(task)
        );
        if (columnIndex !== -1) {
          const column = this.columns[columnIndex];
          const taskIndex = column.tasks.indexOf(task);
          if (taskIndex !== -1) {
            column.tasks.splice(taskIndex, 1);
          }
        }
      }
    },
  },
  mounted() {
    if (process.client) {
      this.$nextTick(() => {
        draggable();
      });
    }
  },
};
</script>

<style scoped>
.drag-container {
  min-height: 100px;
}
</style>
