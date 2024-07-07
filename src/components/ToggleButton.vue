<!-- ToggleButton.vue -->
<script setup>

const props = defineProps(['tipo', 'isVisible', 'toggleVisibility'])

// Method to toggle visibility
const toggle = () => {
    props.toggleVisibility();
};
</script>

<template>
    <div @click="toggle">
        <div class="flex flex-row items-baseline gap-3 cursor-pointer">
            <slot />
            <button class="text-moderate-blue hover:text-light-grayish-blue">{{ isVisible ? "Cancel" : tipo || 'Reply' }}</button>
        </div>
    </div>
</template>

