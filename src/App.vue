<script setup>
import { ref, onMounted } from 'vue';

const tasksModel = ref({
  tasks: [],
  editing: null,
});

const createTask = (data, form$) => {
  addToStorage(form$.data)
  syncFromStorage()

  form$.reset()
  // form$.clear()
};

const addToStorage = (data) => {
  let storageData = localStorage.getItem("tasks")
  storageData = storageData ? JSON.parse(storageData) : []

  storageData.push(data)
  localStorage.setItem('tasks', JSON.stringify(storageData))
};

const syncFromStorage = () => {
  let tasks = localStorage.getItem('tasks')

  tasksModel.value = {
    tasks: tasks ? JSON.parse(tasks) : []
  }
};

const syncToStorage = () => {
  localStorage.setItem('tasks', JSON.stringify(tasksModel.value.tasks))
};

const edit = (index) => {
  tasksModel.value.editing = index
};

const cancel = () => {
  tasksModel.value.editing = null
  syncFromStorage()
};

const save = () => {
  syncToStorage()
  tasksModel.value.editing = null
};

onMounted(() => {
  syncFromStorage()
});
</script>

<template>
  <h1>Hello, World!</h1>
</template>

<style lang="scss">
.page {
	background: #f1f5f9;
	width: 100%;
	min-height: 100vh;
	padding-top: 2rem;
}

.container {
	max-width: 600px;
	margin: 0 auto;
}

h1 {
	font-size: 48px;
	margin-bottom: 2rem;
	font-weight: 600;
}

.divider {
	margin: 2rem 0;
	border-color: #e2e8f0;
}

.task-container {
	background: #ffffff;
	padding: 1rem;

	&.is-personal {
		border-left: 3px solid green;
	}

	&.is-business {
		border-left: 3px solid purple;
	}
}

.task-wrapper {
	display: flex;
	align-items: center;
}

.vf-list-handle.task-sort-handle {
	top: 1rem;
}
</style>
