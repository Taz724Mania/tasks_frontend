<template>
  <div>
    <h1>Task Manager :</h1>
    <button @click="toggleAddForm">Add a Task</button>
    <ul>
      <li v-for="task in tasks" :key="task.id">
        <h2>{{ task.title }}</h2>
        <h3>{{ task.details }}</h3>
        <h4>{{ task.dueDateTime }}</h4>
        <h4>{{ task.completed }}</h4>
      </li>
    </ul>
    <button>Edit this Task</button>
    <button>Delete this Task</button>
    <FormView v-if="showAddForm" :addTask="addTask" :handleAddTask="handleAddTask"/>
  </div>
</template>



<script setup>
import { ref, onMounted } from 'vue'
import FormView from "./components/AddFormView.vue"

const url = 'https://tasks-backend-bws4.onrender.com/task/'
const tasks = ref([])
const showAddForm = ref(false)
const newTask = ref({
    title: '',
    details: '',
    dueDateTime: '',
    completed: false,
    id: null
  });


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
    showAddForm.value = false;
  } catch (error) {
    console.error('Could not add task:', error);
  }
};

const editTask = async (task) => {
  try {
    const response = await fetch(`${url}${task.id}`, {
      method: 'PUT',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify(task),
    });

    if (response.ok) {
      console.log('Task updated successfully!');
    } else {
      console.error('Error updating task:', await response.text());
    }
  } catch (error) {
    console.error('Error saving task:', error);
  }
};


const deleteTask = async (taskId) => {
  try {
    const response = await fetch(`${url}${taskId}`, { method: 'DELETE' });

    if (response.ok) {
      tasks.value = tasks.value.filter((task) => task.id !== taskId);
    } else {
      console.error('Could not Delete task', await response.text());
    }
  } catch (error) {
    console.error('Could not Delete task', error);
  }
};

const toggleAddForm = () => {
  showAddForm.value = !showAddForm.value
}

const handleAddTask = (newTaskData) => {
  addTask(newTaskData); 
}

onMounted(getTasks)
</script>




<style>

</style>