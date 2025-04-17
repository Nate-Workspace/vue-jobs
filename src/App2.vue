<script setup>
import { onMounted, ref } from 'vue'

const name = ref('Nate')
const status = ref('active')
const tasks = ref(['Task one', 'Task two', 'Task three'])
const newTask = ref('')

const toggleStatus = () => {
  if (status.value === 'active') {
    status.value = 'in_progress'
  } else if (status.value === 'in_progress') {
    status.value = 'inactive'
  } else {
    status.value = 'active'
  }
}

const onSubmit = () => {
  if (newTask.value.trim !== '') {
    tasks.value.push(newTask.value)
    newTask.value = ''
  }
}

const onDelete = (index) => {
  tasks.value.splice(index, 1)
}

onMounted(async () => {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/todos')
    const data = await response.json()
    tasks.value = data.map((task) => task.title)
  } catch (error) {
    console.log('Error loading the data')
  }
})

console.log(status)
</script>

<template>
  <h1>{{ name }}</h1>
  <p v-if="status === 'active'">Nate is active</p>
  <p v-else-if="status === 'in_progress'">Nate is in progress</p>
  <p v-else>Nate is not active</p>

  <form @submit.prevent="onSubmit">
    <label for="newTask">New Task</label>
    <input type="text" id="newTask" name="newTask" v-model="newTask" />
    <button type="submit">Add</button>
  </form>
  <h3>Tasks:</h3>
  <ul>
    <li v-for="(task, index) in tasks" :key="task">
      <span>{{ task }}</span>
      <button @click="onDelete(index)">X</button>
    </li>
  </ul>
  <button @click="toggleStatus">toggle status</button>
</template>

<!-- export default {
  data() {
    return {
      name: 'Nathan I',
      status: true,
      tasks: ['Task one', 'Task two', 'task three'],
    }
  },
  methods: {
    toggleStatus(){
      if(this.status)
    }
  }
} -->
