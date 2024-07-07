<!-- ToggledElement.vue -->
<script setup>
import { inject, onMounted, onUnmounted } from 'vue';
import { eventBus } from './eventBus';

// Inject the state to control visibility and the toggle method
// const isVisible = inject('isVisible');
// const toggleVisibility = inject('toggleVisibility');

// onMounted(() => {
//   eventBus.on('toggle', toggleVisibility);
// });

// onUnmounted(() => {
//   eventBus.off('toggle', toggleVisibility);
// });

const props = defineProps(['isVisible', 'toggleVisibility']);

</script>

<template>
    <div v-if="isVisible">
        <div @toggle="toggleVisibility">
            <slot></slot>
        </div>
        
    </div>
</template>
