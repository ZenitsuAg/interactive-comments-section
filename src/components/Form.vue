<script setup>
import { ref } from 'vue';


const emits = defineEmits(['handleCommentSubmit'])

const newComment = ref('')

function handleCommentSubmit() {
    if (newComment.value == "") {
        return;
    } else {
        emits('handleCommentSubmit', newComment.value)
        newComment.value = ''
    }
}

</script>

<template>
    <!-- Mobile form -->
    <form @submit.prevent="handleCommentSubmit" class="flex flex-col items-start gap-5 p-4 m-3 bg-white md:hidden">
        <textarea 
        rows="4" 
        placeholder="Add a comment..."
        v-model="newComment"
        class="w-full p-3 border-2 rounded-lg outline-none resize-none border-dark-blue focus:outline-2 focus:outline-moderate-blue"></textarea>
        <div class="flex justify-between w-full">
            <slot></slot>
            <input
            class="p-3 font-medium text-white rounded-lg bg-moderate-blue hover:bg-light-grayish-blue disabled:bg-light-grayish-blue"
            type="submit"
            value="SEND"
            >
        </div>
    </form>

    <!-- Desktop form -->

    <form @submit.prevent="handleCommentSubmit" class="items-start hidden gap-6 p-5 mx-5 bg-white rounded-lg md:flex">
        <slot></slot>
        <textarea 
        rows="3" 
        placeholder="Add a comment..."
        v-model="newComment"
        class="w-full p-3 border rounded-lg outline-none resize-none border-light-grayish-blue focus:outline-2 focus:outline-moderate-blue"></textarea>
        <input
            class="p-3 px-6 font-medium text-white rounded-lg bg-moderate-blue hover:bg-light-grayish-blue disabled:bg-light-grayish-blue"
            type="submit"
            value="SEND"
        >
        
    </form>
</template>