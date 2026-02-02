<script lang="ts" setup>
import type { Task } from '../types';

const props = defineProps<{
	tasks: Task[];
}>();

const emits = defineEmits<{
	toggleCompleted: [id: string];
	removeTask: [id: string];
}>();
</script>
<template>
	<TransitionGroup name="list"
					 tag="div">
		<article v-for="task in tasks"
				 :key="task.id"
				 class="the-task">
			<label>
				<input type="checkbox"
					   :checked="task.completed"
					   @change="emits('toggleCompleted', task.id)" />
				<span :class="{ 'task-completed': task.completed }">
					{{ task.title }}
				</span>
			</label>
			<button class="outline"
					@click="emits('removeTask', task.id)">Remove</button>
		</article>
	</TransitionGroup>
</template>

<style scoped>
.task-completed {
	text-decoration: line-through;
	color: gray;
}

.the-task {
	display: flex;
	justify-content: space-between;
	align-items: center;
	padding: 1rem;
}

.list-enter-active,
.list-leave-active {
  transition: all 0.3s ease;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateX(300px);
}
</style>