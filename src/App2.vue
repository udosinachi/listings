<script setup>
import { ref, onMounted } from "vue";

const name = ref("John Doe");
const status = ref("pending");
const tasks = ref(["task one", "task two", "task three"]);
const link = ref("https://google.com");
const newTask = ref("");

const toggleStatus = () => {
  if (status.value === "active") {
    status.value = "pending";
  } else if (status.value === "pending") {
    status.value = "inactive";
  } else {
    status.value = "active";
  }
};

const addTask = () => {
  if (newTask.value.trim !== "") {
    tasks.value.push(newTask.value);
    newTask.value = "";
  }
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
};

onMounted(async () => {
  try {
    const response = await fetch("https://jsonplaceholder.typicode.com/todos");
    const data = await response.json();
    tasks.value = data.map((task) => task.title);
  } catch (error) {
    console.log("Error fetching tasks");
  }
});
</script>

<template>
  <h1>{{ name }}</h1>
  <p v-if="status === 'active'">This user is active</p>
  <p v-else-if="status === 'pending'">This user is pending</p>
  <p v-else>This user is not active</p>

  <form @submit.prevent="addTask">
    <label for="newTask">Add Task</label>
    <input type="text" id="newTask" name="newTask" v-model="newTask" />
    <button type="submit">Submit</button>
  </form>

  <li v-for="(task, index) in tasks" :key="task">
    <span>{{ task }}</span>
    <button @click="deleteTask(index)">x</button>
  </li>
  <a :href="link">Click me</a>
  <button @click="toggleStatus">Toggler</button>
</template>
