<script setup>
import { ref } from 'vue'

const tasks = ref([])
const newTask = ref('')

function addTask() {
  if (newTask.value.trim() !== '') {
    tasks.value.push({
      id: Date.now(),
      text: newTask.value,
      completed: false
    })
    newTask.value = ''
    console.log(tasks.value)
  }
}

const toggleDone = (task) => {
  task.completed == !task.completed
  console.log(task.completed)
}

</script>

<template>
  <input type="text" v-model="newTask" @keyup.enter="addTask">
  <button @click="addTask">Add Task</button>

  <ul>
    <li v-for="task in tasks" :key="task.id">
      <input type="checkbox" v-model="task.completed" @change="toggleDone(task)">
      {{ task.text }}
    </li>
  </ul>
</template>

<style scoped></style>
