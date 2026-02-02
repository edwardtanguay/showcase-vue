<script lang="ts" setup>
import { computed, ref } from "vue";
import TaskForm from "./components/TaskForm.vue";
import type { Task, TaskFilter } from "./types";
import TaskList from "./components/TaskList.vue";
import FilterButton from "./components/FilterButton.vue";

const title = ref("Vue Showcase");
const tasks = ref<Task[]>([]);
const filter = ref<TaskFilter>("all");

function addTask(title: string) {
	const newTask = {
		id: crypto.randomUUID(),
		title,
		completed: false,
	}
	tasks.value.push(newTask);
}

function toggleCompleted(id: string) {
	const task = tasks.value.find(t => t.id === id);
	if (task) {
		task.completed = !task.completed;
	}
}

const totalCompleted = computed(() => {
	return tasks.value.filter(t => t.completed).length;
});

const filteredTasks = computed(() => {
	switch (filter.value) {
		case "all":
			return tasks.value;
		case "todo":
			return tasks.value.filter(t => !t.completed);
		case "completed":
			return tasks.value.filter(t => t.completed);
		default:
			return tasks.value;
	}
});

function removeTask(id: string) {
	tasks.value = tasks.value.filter(t => t.id !== id);
}

function setFilter(newFilter: TaskFilter) {
	filter.value = newFilter;
}

</script>

<template>
	<main>
		<h1>{{ title }}</h1>
		<TaskForm @add-task="addTask" />

		<p v-if="tasks.length <= 0">Add a task to get started.</p>
		<p v-else> {{ totalCompleted }} / {{ tasks.length }} tasks completed:</p>
		<div v-if="tasks.length > 0"
			 class="button-container">
			<FilterButton :currentFilter="filter" filter="all"
						  @set-filter="setFilter" />
			<FilterButton :currentFilter="filter" filter="todo"
						  @set-filter="setFilter" />
			<FilterButton :currentFilter="filter" filter="completed"
						  @set-filter="setFilter" />
		</div>
		<div class="task-list">
			<TaskList :tasks="filteredTasks"
					  @toggle-completed="toggleCompleted"
					  @remove-task="removeTask" />
		</div>
	</main>
</template>

<style scoped>
main {
	max-width: 800px;
	margin: 1rem auto;
}

.task-list {
	margin-top: 1.5rem;
}

.button-container {
	display: flex;
	justify-content: end;
	gap: 0.5rem;
	margin-top: 1rem;
}
</style>
