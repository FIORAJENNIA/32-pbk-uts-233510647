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
</script>

<template>
  <input type="text" v-model="newTask" @keyup.enter="addTask">
  <button @click="addTask">Add Task</button>
</template>

<style scoped>

</style>
