<template>
    <div>
      <h2>Add a Task</h2>
      <form @submit.prevent="handleSubmit">
        <label for="title">Title:</label>
            <input type="text" id="title" v-model="newTask.title" required />
        <label for="details">Details:</label>
            <textarea id="details" v-model="newTask.details"></textarea>
        <label for="dueDateTime">Due Date and Time:</label>
            <input type="datetime-local" id="dueDateTime" v-model="newTask.dueDateTime" />
        <label for="completed">Completed: </label>
            <input type="checkbox" id="checkbox">
        <button type="submit">Submit</button>
      </form>
    </div>
  </template>
  
  <script setup>
  import { ref, defineEmits } from 'vue';
  
  const formProps = {
    addTask: Function,
    showForm: Boolean,
  };
  
  const newTask = ref({
    title: '',
    details: '',
    dueDateTime: '',
    completed: false,
    id: null
  });
  
  const emitSubmit = defineEmits(['submitForm'])
  const showAddForm = ref(false)

  const handleSubmit = async (e) => {
    e.preventDefault();
    emitSubmit('submitForm', newTask.value)
    showAddForm.value = false
  };

  </script>
  
  
  