<script setup>
import { ref, computed } from 'vue'

const tasks = ref([])
const newTask = ref('')
const filter = ref('all')

const addTask = () => {
  if (newTask.value !== '') {
    tasks.value.push({
      id: Date.now(),
      text: newTask.value,
      completed: false
    })
    newTask.value = ''
  }
}

const removeTask = (task) => {
  tasks.value = tasks.value.filter(t => t.id !== task.id)
}

const toggleTask = (task) => {
  task.completed == !task.completed
}

const filteredTasks = computed(() => {
  if (filter.value === 'completed') {
    return tasks.value.filter(task => task.completed)
  } else if (filter.value === 'active') {
    return tasks.value.filter(task => !task.completed)
  } else {
    return tasks.value
  }
})

const activeBtn = 'bg-blue-600 text-white px-4 py-2 rounded-md shadow transition'
const inactiveBtn = 'bg-white/10 text-white px-4 py-2 rounded-md hover:bg-white/20 transition'
</script>
<template>
  <div class="min-h-screen bg-gradient-to-br from-slate-900 to-slate-700 text-white p-6">
    <div class="max-w-3xl mx-auto bg-white/10 backdrop-blur-md rounded-xl shadow-lg p-6 space-y-4 text-white">

      <h1 class="text-3xl font-bold">Task Manager</h1>

      <div class="flex flex-col sm:flex-row gap-3">
        <input type="text" v-model="newTask" @keyup.enter="addTask" placeholder="Tambahkan tugas baru..."
          class="flex-1 p-3 rounded-md text-white placeholder-gray-400 focus:outline-none focus:ring-2 focus:ring-blue-500" />
        <button @click="addTask"
          class="bg-blue-600 hover:bg-blue-700 transition text-white px-4 py-2 rounded-md shadow">
          Tambahkan
        </button>
      </div>

      <div class="flex flex-wrap gap-2 mt-2">
        <button @click="filter = 'all'" :class="filter === 'all' ? activeBtn : inactiveBtn">
          Semua
        </button>
        <button @click="filter = 'active'" :class="filter === 'active' ? activeBtn : inactiveBtn">
          Aktif
        </button>
        <button @click="filter = 'completed'" :class="filter === 'completed' ? activeBtn : inactiveBtn">
          Selesai
        </button>
      </div>
      
      <ul class="max-h-[45vh] h-45 sm:h-60 md:h-80 lg:h-100 overflow-y-auto space-y-3 pr-1 custom-scroll">
        <li v-for="task in filteredTasks" :key="task.id"
          class="bg-white/20 p-3 rounded-md flex justify-between items-center hover:bg-white/30 transition">
          <div class="flex items-center gap-3">
            <input type="checkbox" v-model="task.completed" @change="toggleTask(task)"
              class="accent-blue-500 w-5 h-5" />
            <span :class="{ 'line-through text-gray-300': task.completed }">{{ task.text }}</span>
          </div>
          <button @click="removeTask(task)" class="text-red-400 hover:text-red-600 transition">
            ✕
          </button>
        </li>
      </ul>

    </div>
  </div>
</template>


<style scoped>
html,
body {
  overflow: hidden;
}

.custom-scroll::-webkit-scrollbar {
  width: 6px;
}

.custom-scroll::-webkit-scrollbar-thumb {
  background-color: rgba(255, 255, 255, 0.4);
  border-radius: 4px;
}

.custom-scroll {
  scrollbar-width: thin;
  scrollbar-color: rgba(255, 255, 255, 0.4) transparent;
}
</style>
