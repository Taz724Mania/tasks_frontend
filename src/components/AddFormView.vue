<template>
    <div>
      <form @submit.prevent="handleSubmit">
        <label for="title">Title:</label>
            <input type="text" id="title" v-model="newTask.title" required />
        <label for="details">Details:</label>
            <textarea id="details" v-model="newTask.details"></textarea>
        <label for="dueDateTime">Due Date and Time:</label>
            <input type="datetime-local" id="dueDateTime" v-model="newTask.dueDateTime" />
        <label>Completed:</label>
          <input type="checkbox" id="completed" v-model="newTask.completed">
        
      </form>
    </div>
  </template>
  
  <script setup>
    import {ref, defineProps} from 'vue'

    const title = ref(props.title)
    const details = ref(props.details)  
    const dueDateTime = ref(props.dueDateTime)
    const completed = ref(props.completed)
    const id = ref(props.id)


    const props = defineProps({
      title: String,
      details: String,
      dueDateTime: String,
      completed: Boolean,
      _id: String
    })

  const emitSubmit = defineEmits(['submitForm'])
  const showAddForm = ref(false)

  const handleSubmit = async (e) => {
    e.preventDefault();
    await emitSubmit('submitForm', newTask.value)
    showAddForm.value = false
  };

    console.log(props)
  </script>
  
  
  