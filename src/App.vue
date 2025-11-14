<script setup>
import { ref } from 'vue';
import Task from './components/Task.vue';

const tasks = ref([
  { id: 1, title: 'Learn Vue.js', completed: false },
  { id: 2, title: 'Build a Vue app', completed: true },
  { id: 3, title: 'Deploy the app', completed: false },
]);


</script>

<template>
  <h1>Vue Todo</h1>
  <ul v-if="tasks.length">
    <li v-for="task in tasks">
      <Task 
        :title="task.title" 
        :completed="task.completed" 
        @toggle="() => task.completed = !task.completed" 
        @delete="() => tasks = tasks.filter(t => t.id !== task.id)"
      />
    </li>
  </ul>
  <p v-else>
    No tasks available.
  </p>
</template>

<style scoped></style>
