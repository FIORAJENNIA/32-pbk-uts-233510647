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
  }
}

const toggleDone = (task) => {
  task.completed == !task.completed
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
  <div class="min-h-screen w-full bg-gradient-to-r from-pink-100 via-rose-50 to-pink-100 flex flex-col items-center p-6">
    <div class="w-full max-w-lg">
      <div class="flex items-center justify-center mb-8 animate-pulse">
        <svg xmlns="http://www.w3.org/2000/svg" class="h-8 w-8 text-pink-500" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round">
          <path d="M12 2s8 5 8 10c0 5-8 10-8 10-8 0-8-5-8-10 0-5 8-10 8-10z"></path>
          <path d="M12 12a3 3 0 100-6 3 3 0 000 6z"></path>
          <line x1="12" y1="16" x2="12" y2="21"></line>
        </svg>
        <h1 class="text-3xl font-bold text-pink-600 ml-2 font-serif">Lovely Tasks</h1>
      </div>

      <div class="mb-6 flex space-x-2 w-full">
        <input 
          type="text" 
          v-model="newTask" 
          @keyup.enter="addTask"
          placeholder="Add a new task..." 
          class="flex-grow px-4 py-3 rounded-lg border border-pink-200 focus:outline-none focus:ring-2 focus:ring-pink-300 transition-all bg-white shadow-sm"
        >
        <button 
          @click="addTask"
          class="bg-pink-500 hover:bg-pink-600 text-white px-4 py-2 rounded-lg transition-all shadow-sm transform hover:scale-105"
        >
          Add
        </button>
      </div>

      <div class="mb-4 bg-white p-4 rounded-lg shadow-sm border border-pink-100">
        <select 
          v-model="filtered"
          class="w-full p-2 rounded-md border border-pink-200 focus:outline-none focus:ring-2 focus:ring-pink-300 bg-transparent text-gray-700"
        >
          <option value="all">All Tasks</option>
          <option value="completed">Completed</option>
          <option value="incomplete">Incomplete</option>
        </select>
      </div>

      <div class="bg-white rounded-lg shadow-sm border border-pink-100 p-4 h-[40vh] overflow-hidden">
        <div class="h-full overflow-y-auto">
          <ul v-if="filteredTasks.length > 0" class="divide-y divide-pink-100">
            <li 
              v-for="task in filteredTasks" 
              :key="task.id"
              class="py-3 px-2 flex items-center group transition-all hover:bg-pink-50 rounded-md"
            >
              <input 
                type="checkbox" 
                v-model="task.completed" 
                @change="toggleDone(task)"
                class="h-5 w-5 rounded-full border-pink-300 text-pink-500 focus:ring-pink-200 mr-3 cursor-pointer"
              >
              <span 
                :class="[
                  'flex-grow text-gray-700', 
                  task.completed ? 'line-through text-gray-400' : ''
                ]"
              >
                {{ task.text }}
              </span>
              <button 
                @click="deleteTask(task)"
                class="opacity-0 group-hover:opacity-100 bg-pink-100 hover:bg-pink-200 text-pink-600 rounded-full p-1 ml-2 transition-all"
              >
                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" viewBox="0 0 20 20" fill="currentColor">
                  <path fill-rule="evenodd" d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z" clip-rule="evenodd" />
                </svg>
              </button>
            </li>
          </ul>
          <div v-else class="text-center py-6 text-gray-500 italic">
            No tasks to display
          </div>
        </div>
      </div>

      <div class="mt-4 text-center text-sm text-pink-400">
        <p>@FIORA JENNIA</p>
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>