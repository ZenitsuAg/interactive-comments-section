<script setup>
import { ref } from 'vue';
import { eventBus } from './eventBus';

// const emitToggle = () => {
//   eventBus.emit('toggle');
// };

const props = defineProps({
  initialReply: {
    type: String,
    default: ''
  }
})

const emit = defineEmits(['submitReply', 'toggle']);


const replyText = ref(props.initialReply)

const handleSubmit = () => {
  emit('submitReply', replyText.value)
  replyText.value = ''
  emit('toggle')
}

</script>

<template>
    <div>
        <form @submit.prevent="handleSubmit" class="flex items-start gap-7">
            <slot />
            <textarea
            class="w-full p-3 border-2 rounded-lg resize-none border-dark-blue"
            type="text"
            placeholder="Reply..."
            v-model="replyText"
            rows="4"
            ></textarea>
        
            <input
            class="p-3 font-medium text-white rounded-lg bg-moderate-blue hover:bg-light-grayish-blue disabled:bg-light-grayish-blue"
            type="submit"
            value="REPLY"
            :disabled="!replyText"
            >
        </form>        
        <!-- <button @submit="emitToggle">Toggle</button> -->
    </div>
</template>