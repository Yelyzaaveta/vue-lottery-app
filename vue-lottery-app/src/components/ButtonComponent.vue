<template>
  <button
    @click="$emit('click')"
    :disabled="disabled"
    class="bg-blue-500 text-white px-4 py-2 rounded disabled:opacity-50 shadow-md"
  >
    New winner
  </button>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'ButtonComponent',
  props: {
    disabled: {
      type: Boolean,
      default: false
    }
  }
});
</script>
