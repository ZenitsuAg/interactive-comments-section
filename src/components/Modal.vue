<script setup>
import { ref } from 'vue';
import deleteIcon from '../../images/icon-delete.svg'

const emits = defineEmits(['handleDelete'])

const handleDelete = () => {
    emits('handleDelete')
    openModal.value=!openModal.value
}

const openModal = ref(false)
const showModal = () => {

}
</script>

<template>
    <div @click="openModal=!openModal" class="flex flex-row items-center gap-3">
        <img :src="deleteIcon" alt="delete icon">
        <button class="text-soft-red hover:text-pale-red">Delete</button>
    </div>
    <div v-if="openModal">
        <div class=" h-screen fixed text-white z-[1] left-0 top-0 w-full overflow-auto bg-black bg-opacity-30 flex items-center justify-center">
            <div @click.stop="" class="bg-white rounded-md p-7 text-slate-900 max-w-96">
                <h1 class="text-2xl text-dark-blue">Delete Comment</h1>
                <p class="my-5 font-normal text-grayish-blue">Are you sure you want to delete this comment? This will remove the comment and can't be undone</p>
                <div class="flex justify-between">
                    <button class="py-3 text-white px-7 rounded-xl bg-grayish-blue hover:bg-grayish-blue/70" @click="openModal=!openModal">NO, CANCEL</button>
                    <button class="py-3 text-white px-7 rounded-xl bg-soft-red hover:bg-pale-red" @click="handleDelete">YES, DELETE</button>
                </div>
            </div>
        </div>
    </div>
</template>