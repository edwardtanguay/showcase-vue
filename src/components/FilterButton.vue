<script lang="ts" setup>
import type { TaskFilter } from '../types';

const props = defineProps<{
	filter: TaskFilter;
	currentFilter: TaskFilter;
}>();

const emits = defineEmits<{
	setFilter: [filter: TaskFilter];
}>();
</script>
<template>
	<button class="secondary"
			@click="emits('setFilter', props.filter)"
			:class="{
				contrast: props.filter === props.currentFilter
			}">{{ props.filter }}
	</button>
</template>

<style scoped>
button {
	padding: 0.5rem 1rem;
	text-transform: capitalize;
}
</style>
