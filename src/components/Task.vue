<script setup>
import { defineProps, defineEmits } from 'vue';

const props = defineProps({
  title: {
    type: String,
    required: true
  },
  completed: {
    type: Boolean,
    default: false
  }
});

const emit = defineEmits(['toggle', 'delete']);

</script>

<template>
  <div :class="['task', { 'completed': completed }]">
    <input type="checkbox" :checked="completed" @change="emit('toggle')" />
    <span class="title">
      {{ title }}
    </span>
    <button class="delete-btn" @click="emit('delete')">Delete</button>
  </div>
</template>

<style scoped>
.task {
  padding: 10px;
  border-bottom: 1px solid #eee;
  display: flex;
  align-items: center;
}

.title {
  margin-left: 10px;
  font-size: 16px;
}

.completed .title {
  text-decoration: line-through;
  color: #888;
}

.delete-btn {
  margin-left: auto;
  background: transparent;
  border: none;
  color: red;
  cursor: pointer;
}
</style>