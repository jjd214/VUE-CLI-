<script setup>
import { onMounted, ref } from 'vue'

const newTask = ref('')
const tasks = ref([])
const isEdit = ref(false)
const editIndex = ref(null)

const saveTaskHandler = () => {
  if (newTask.value.trim() === '') return

  if (!isEdit.value) {
    tasks.value.push(newTask.value)
  } else {
    tasks.value[editIndex.value] = newTask.value
    isEdit.value = false
    editIndex.value = null
  }

  newTask.value = ''
}

const editTask = (index) => {
  newTask.value = tasks.value[index]
  isEdit.value = true
  editIndex.value = index
}

const deleteTask = (index) => {
  tasks.value.splice(index, 1)
}

onMounted(async () => {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/todos')
    const data = await response.json()
    tasks.value = data.slice(0, 10).map((task) => task.title)
  } catch (error) {
    console.log('Error fetching task.')
  }
})
</script>

<template>
  <div>
    <div class="row">
      <div class="col-md-6">
        <div class="form-group">
          <form @submit.prevent="saveTaskHandler">
            <label for="" class="form-label">Task: </label>
            <input type="text" class="form-control" v-model="newTask" />
            <button type="submit" class="btn btn-primary">Save Task</button>
          </form>
        </div>
      </div>
    </div>

    <table class="table table-dark table-hover">
      <thead>
        <tr>
          <th>#</th>
          <th>Task</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td>{{ index + 1 }}</td>
          <td>{{ task }}</td>
          <td>
            <a @click="editTask(index)" class="btn btn-outline-info">Edit</a>
            <a @click="deleteTask(index)" class="btn btn-outline-danger">Delete</a>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped></style>
