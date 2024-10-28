<template>
  <span class="bg-blue-500 text-white py-1 px-3 mr-2 rounded">
    {{ winner.name }} <button @click="$emit('removeWinner')" class="ml-2 text-red-500">x</button>
  </span>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'WinnerComponent',
  props: {
    winner: {
      type: Object as () => { name: string },
      required: true
    }
  }
});
</script>
