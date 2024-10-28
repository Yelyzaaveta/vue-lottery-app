<template>
    <div v-if="isOpen" class="fixed inset-0 flex items-center justify-center bg-gray-800 bg-opacity-50 z-50">
      <div class="bg-white rounded-lg p-6 w-[400px] shadow-md">
        <h2 class="text-lg font-semibold mb-4">{{ title }}</h2>
        <slot></slot>
        <div class="flex justify-end space-x-4 mt-4">
          <button @click="close" class="bg-gray-300 text-gray-700 px-4 py-2 rounded hover:bg-gray-400">Cancel</button>
          <button @click="confirm" :class="confirmButtonClass">{{ confirmButtonText }}</button>
        </div>
      </div>
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent } from 'vue';
  
  export default defineComponent({
    props: {
      isOpen: {
        type: Boolean,
        required: true,
      },
      title: {
        type: String,
        default: 'Edit Participant',
      },
      confirmButtonText: {
        type: String,
        default: 'Save',
      },
      confirmButtonClass: {
        type: String,
        default: 'bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600',
      },
    },
    emits: ['close', 'confirm'],
    methods: {
      close() {
        this.$emit('close');
      },
      confirm() {
        this.$emit('confirm');
      },
    },
    
  });
  </script>
  