<template>
  <div class="mb-4">
    <label class="block text-gray-700">{{ label }}</label>
    <input
      v-model="modelValue"
      :type="type"
      class="w-full p-2 border"
      :class="inputClass"
      :placeholder="placeholder"
      @input="$emit('update:modelValue', modelValue)"
    />
    <p v-if="error" class="text-red-500 text-sm">{{ error }}</p>
  </div>
</template>

<script lang="ts">
import { defineComponent, computed } from 'vue';

export default defineComponent({
  name: 'InputFieldComponent',
  props: {
    modelValue: String,
    label: String,
    type: {
      type: String,
      default: 'text',
    },
    error: String,
    submitted: Boolean,
    placeholder: String,
  },
  setup(props) {
    const inputClass = computed(() => {
      if (!props.submitted) return 'border-gray-400';
      return props.error ? 'border-red-500' : 'border-green-500';
    });

    return { inputClass };
  },
});
</script>
