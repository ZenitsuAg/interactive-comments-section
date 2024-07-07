<script setup>
import { ref, defineProps } from 'vue';
import plusIcon from '../../images/icon-plus.svg'
import minusIcon from '../../images/icon-minus.svg'

const props = defineProps({
    score: Number
})

const emit = defineEmits(['increaseNumber', 'decreaseNumber'])

const newNumber = ref(props.score)

function increaseNumber() {
    emit('increaseNumber')
}

function decreaseNumber() {
    emit('decreaseNumber')
}

</script>

<template>
    <div class="flex justify-between w-20 p-2 text-center rounded-lg lg:flex-col lg:w-10 lg:font-medium text-moderate-blue bg-very-light-gray">
        <div @click="increaseNumber" class="my-auto lg:mx-auto lg:p-1">
            <img :src="plusIcon" alt="plus icon">
        </div>
        <p class="px-3 lg:py-3 lg:px-0">{{ score }}</p>
        <div @click="decreaseNumber" class="my-auto lg:mx-auto lg:p-1">
            <img :src="minusIcon" alt="minus icon">
        </div>
    </div>

</template>