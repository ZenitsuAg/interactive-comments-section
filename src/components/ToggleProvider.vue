<!-- ToggleProvider.vue -->
<script setup>
import { provide, ref } from "vue";

// const props = defineProps({
//     uniqueKey: {
//     type: Number,
//     required: true,
//   },
// })
// Create a reactive state for the toggle
// const isVisible = ref(false);

// Provide the state and a method to toggle it
/* provide("isVisible", isVisible);
provide("toggleVisibility", () => {
    isVisible.value = !isVisible.value;
}); */


const isVisible = ref(false);

const toggleVisibility = () => {
  isVisible.value = !isVisible.value;
};
</script>

<template>
    <slot :isVisible="isVisible" :toggleVisibility="toggleVisibility"></slot>
</template>
