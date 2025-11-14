<script setup>
import { ref, watch } from 'vue';
import Task from './components/Task.vue';

const tasks = ref([]);
const newTaskTitle = ref('');

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

function createTask() {
  if (newTaskTitle.value.trim() === '') return;

  const newTask = {
    id: Date.now(),
    title: newTaskTitle.value.trim(),
    completed: false,
  };
  tasks.value.push(newTask);
  newTaskTitle.value = '';
  saveTasksToLocalStorage();
}

</script>

<template>
  <h1>Vue Todo</h1>
  <form @submit.prevent>
    <input 
      v-model="newTaskTitle" 
      type="text" 
      placeholder="Add a new task" 
      required 
    />
    <button type="submit" @click="createTask">
      Add Task
    </button>
  </form>
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
