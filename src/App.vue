<template>
  <div>
    <h1>Task Manager :</h1>
    <button> Add a Task</button>
    <h2>{{ task.title }}</h2>
    <h2>{{ task.details }}</h2>
    <h2>{{ task.dueDateTime }}</h2>
    <h2>{{ task.completed }}</h2>



  </div>
</template>



<script setup>
import { ref, onMounted } from 'vue'
import Form from './components/FormView.vue'

const url = 'https://tasks-backend-bws4.onrender.com/task/'
const tasks = ref([])

const getTasks = async () => {
  try {
    const response = await fetch(url)
    const data = await response.json()
    tasks.value = data
  } catch (error) {
    console.log("Tasks not Found", error)
  }
}

const addTask = async (newTask) => {
  try {
    const response = await fetch(url, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(newTask),
    });

    const data = await response.json();
    tasks.value.push(data);
    showForm.value = false;
  } catch (error) {
    console.error('Error adding task:', error);
  }
};





onMounted(getTasks)
</script>




<style>

</style>