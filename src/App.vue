<script setup>
import { ref } from 'vue'

const tasks = ref([])
const newTask = ref('')

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

</script>

<template>
  <div>
    <input type="text" v-model="newTask" @keyup.enter="addTask">
    <button @click="addTask">Tambahkan</button>
  </div>
</template>

<style scoped></style>
