<script setup>
import { ref, watch } from 'vue';
import Task from './components/Task.vue';

const tasks = ref([]);

watch(
  tasks, 
  (newTasks) => {
    saveTasksToLocalStorage();
  },
);

function saveTasksToLocalStorage() {
  localStorage.setItem('tasks', JSON.stringify(tasks.value));
}

function loadTasksFromLocalStorage() {
  const savedTasks = localStorage.getItem('tasks');
  if (savedTasks) {
    tasks.value = JSON.parse(savedTasks);
  }
}
loadTasksFromLocalStorage();

function handleToggleTaskCompletion(taskId) {
  const task = tasks.value.find(t => t.id === taskId);
  if (task) {
    task.completed = !task.completed;
  }
  saveTasksToLocalStorage();
}

function handleDeleteTask(taskId) {
  tasks.value = tasks.value.filter(t => t.id !== taskId);
  saveTasksToLocalStorage();
}

</script>

<template>
  <h1>Vue Todo</h1>
  <ul v-if="tasks.length">
    <li v-for="task in tasks">
      <Task 
        :title="task.title" 
        :completed="task.completed" 
        @toggle="handleToggleTaskCompletion(task.id)" 
        @delete="handleDeleteTask(task.id)"
      />
    </li>
  </ul>
  <p v-else>
    No tasks available.
  </p>
</template>

<style scoped>
h1 {
  text-align: center;
  margin-bottom: 20px;
  font-family: monospace;
}

ul {
  list-style-type: none;
  padding: 0;
  max-width: 600px;
  margin: 0 auto;
}
</style>
