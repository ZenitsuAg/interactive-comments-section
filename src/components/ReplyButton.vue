<script setup>
import { ref } from 'vue';
import CurrentReply from './CurrentReply.vue';
defineProps({
    srcUser: String,
    message: String,
})

defineEmits({
    createReply: String,
    click: Function,

})
    // const message = ref('')


    let showReply = ref(false)

    const showReplyComponent = () => {
        showReply.value = !showReply.value;
    }
</script>

<template>
    <button @click="showReplyComponent">REPLY</button>

    <div v-if="showReply">
        <img :src="srcUser" alt="hi">
        <!-- <input type="text" v-model="message"> -->
        <slot />
        <button @click="click">REPLY</button>
        <!-- {{ message }} -->
    </div>
    
</template>