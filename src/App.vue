<script setup>
import { ref, watch, computed } from 'vue';
import Task from './components/Task.vue';

const tasks = ref([]);
const newTaskTitle = ref('');

const completedTasks = computed(() => 
  tasks.value.filter(task => task.completed)
);
const pendingTasks = computed(() => 
  tasks.value.filter(task => !task.completed)
);

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
    />
    <button type="submit" @click="createTask">
      Add Task
    </button>
  </form>
  <hr style="max-width: 600px; margin: 0 auto;"/>
  <ul v-if="pendingTasks.length">
    <li v-for="task in pendingTasks">
      <Task 
        :title="task.title" 
        :completed="task.completed" 
        @toggle="handleToggleTaskCompletion(task.id)" 
        @delete="handleDeleteTask(task.id)"
      />
    </li>
  </ul>
  <p v-else style="text-align: center; font-family: monospace;">
    No tasks available.
  </p>
  <details>
    <summary>{{ completedTasks.length }} Completed</summary>
    <ul v-if="completedTasks.length">
      <li v-for="task in completedTasks">
        <Task 
          :title="task.title" 
          :completed="task.completed" 
          @toggle="handleToggleTaskCompletion(task.id)" 
          @delete="handleDeleteTask(task.id)"
        />
      </li>
    </ul>
    <p v-else style="text-align: center; font-family: monospace;">
      No completed tasks.
    </p>
  </details>
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

form {
  display: flex;
  justify-content: center;
  margin: 20px auto;
  gap: 10px;
  max-width: 600px;
}

form input {
  width: 100%;
  padding: 0.5em;
  border: none;
  border-radius: 1em;
}

form button {
  padding: 0.5em 1em;
  border: none;
  border-radius: 1em;
  background-color: #42b983;
  color: white;
  cursor: pointer;
}

details {
  max-width: 600px;
  margin: 20px auto;
  font-family: monospace;
}
</style>
