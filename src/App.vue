<script setup>
import { ref, computed } from 'vue'

const tasks = ref([])
const newTask = ref('')
const filtered = ref('all')

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

const deleteTask = (task) => {
  tasks.value = tasks.value.filter(t => t.id !== task.id)
}

const filteredTasks = computed(() => {
  if (filtered.value === 'all') {
    return tasks.value
  } else if (filtered.value === 'completed') {
    return tasks.value.filter(task => task.completed)
  } else if (filtered.value === 'incomplete') {
    return tasks.value.filter(task => !task.completed)
  }
})

</script>

<template>
  <h1 class="text-red-500">Task Management</h1>
  <input type="text" v-model="newTask" @keyup.enter="addTask">
  <button @click="addTask">Add Task</button>
  <select v-model="filtered">
    <option value="all">All</option>
    <option value="completed">Completed</option>
    <option value="incomplete">Incomplete</option>
  </select>

  <ul>
    <li v-for="task in filteredTasks" :key="task.id">
      <input type="checkbox" v-model="task.completed" @change="toggleDone(task)">
      {{ task.text }}
      <button @click="deleteTask(task)">Delete</button>
    </li>
  </ul>
</template>

<style scoped></style>
