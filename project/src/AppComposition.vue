<script setup>
import { ref, onMounted } from 'vue';

const name = ref('John Doe');
const status = ref('active');
const tasks = ref(['Task 1', 'Task 2', 'Task 3']);
const newTask = ref('fdsafds');

const toggleStatus = () => {
	const { value: previousStatus } = status;

	if (previousStatus === 'active') {
		status.value = 'pending';
	} else if (previousStatus === 'pending') {
		status.value = 'inactive';
	} else {
		status.value = 'active';
	}
};

const addTask = () => {
	if (newTask.value.trim()) {
		tasks.value.push(newTask.value);
		newTask.value = '';
	}
};

const removeTask = (index) => {
	tasks.value.splice(index, 1);
};

onMounted(async () => {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/todos');
    const data = await response.json();
    console.log(data);

    tasks.value = data.map(({title}) => title)
  } catch (error) {
    console.error(error);
  }
});
</script>

<template>
	<h1>{{ name }}</h1>
	<p v-if="status === 'active'">User is active.</p>
	<p v-else-if="status === 'pending'">User is pending.</p>
	<p v-else>User is inactive.</p>

	<form @submit.prevent="addTask">
		<label for="newTask">Add Task</label>
		<input type="text" id="newTask" name="newTask" v-model="newTask" />
		<button type="submit">Submit</button>
	</form>

	<h3>Tasks:</h3>
	<ul>
		<li v-for="(task, index) in tasks" :key="task">
			<span>
				{{ task }}
			</span>
			<button @click="removeTask(index)">X</button>
		</li>
	</ul>

	<br />
	<button v-on:click="toggleStatus">Change Status</button>
</template>
