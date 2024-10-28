<template>
    <div>
      <input 
        type="text" 
        @input="onInput" 
        placeholder="Search participants by name" 
        class="border p-2 rounded w-full mx-[2px] my-[4px] bg-gray-100"
      />
    </div>
  </template>
  
  <script lang="ts">
  import { defineComponent } from 'vue';
  
  export default defineComponent({
    emits: ['filter-by-name'],
    methods: {
      onInput(event: Event) {
        const input = event.target as HTMLInputElement;
        this.$emit('filter-by-name', input.value);
      }
    }
  });
  </script>
  