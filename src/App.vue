<script setup>
  import { ref } from 'vue';

  const tasks = ref([
    { id: 1, title: 'Learn Vue.js', completed: false },
    { id: 2, title: 'Build a Vue app', completed: false },
    { id: 3, title: 'Deploy the app', completed: false },
  ]);


</script>

<template>
  <h1>Vue Tasks</h1>
  <ul v-if="tasks.length">
    
  </ul>
  <p v-else>
    No tasks available.
  </p>
</template>

<style scoped></style>
