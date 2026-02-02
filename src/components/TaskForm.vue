<script lang="ts" setup>
import { ref } from "vue";

const emit = defineEmits<{
  addTask: [title: string];
}>();

const title = ref("");
const error = ref("");

function submitForm() {
  if (title.value.trim() !== "") {
	emit("addTask", title.value.trim());
	title.value = "";
  } else {
	error.value = "Title cannot be empty";
  }
}
</script>

<template>
  <form @submit.prevent="submitForm">
	<label>
	  New Task
	  <input v-model="title" :aria-invalid="!!error || undefined" @input="error = ''" />
	  <small id="invalid-helper">
		{{ error }}
	  </small>
	</label>
	<div class="button-container">
	  <button>Add</button>
	</div>
  </form>
</template>

<style scoped>
.button-container {
  display: flex;
  justify-content: end;
}
</style>
