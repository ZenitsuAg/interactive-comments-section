<script setup>
import { ref, watch, onMounted } from 'vue';

const props = defineProps({
  initialEdit: {
    type: String,
    default: ''
  },
  value: {
    type: String,
    default: ''
  }
});

const emit = defineEmits(['submitEdit', 'update:value', 'toggle']);

const editText = ref('');

onMounted(() => {
  editText.value = props.value || props.initialEdit;
});

watch(() => props.value, (newValue) => {
  if (newValue !== editText.value) {
    editText.value = newValue;
  }
});

const handleSubmit = () => {
  emit('submitEdit', editText.value);
  editText.value = '';
  emit('toggle');
  emit('update:value', ''); // Reset the parent value
};

const handleInput = (event) => {
  editText.value = event.target.value;
  emit('update:value', editText.value);
};

</script>

<template>
  <div>
    <form @submit.prevent="handleSubmit" class="flex items-start gap-7">
      <slot />
      <textarea
        class="w-full p-3 border-2 rounded-lg resize-none border-dark-blue"
        v-model="editText"
        rows="4"
        @input="handleInput"
      ></textarea>
      <input
        class="p-3 font-medium text-white rounded-lg bg-moderate-blue hover:bg-light-grayish-blue disabled:bg-light-grayish-blue"
        type="submit"
        value="EDIT"
        :disabled="!editText"
      >
    </form>
  </div>
</template>
