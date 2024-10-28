<template>
  <div class="mb-4 p-6 shadow-md">
      <div class="flex items-center justify-between border border-gray">
          <div>
              <span v-for="(winner, index) in winners" :key="index"
                  class="bg-blue-500 text-white py-1 px-3 mr-2 rounded">
                  {{ winner.name }} <button @click="removeWinner(index)" class="ml-2 text-red-500">x</button>
              </span>
          </div>
          <button @click="selectWinner" :disabled="winners.length >= 3 || participants.length === 0"
              class="bg-blue-500 text-white px-4 py-2 rounded disabled:opacity-50 shadow-md ">
              New winner
          </button>
      </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import type {  PropType } from 'vue';

export default defineComponent({
  props: {
      winners: {
          type: Array as PropType<{ name: string }[]>,
          required: true,
      },
      participants: {
          type: Array as PropType<{ name: string; birthday: string; email: string; phone: string }[]>,
          required: true,
      },
  },
  emits: ['select-winner', 'remove-winner'],
  setup(props, { emit }) {
      const selectWinner = () => {
          if (props.participants.length > 0 && props.winners.length < 3) {
              const randomIndex = Math.floor(Math.random() * props.participants.length);
              emit('select-winner', props.participants[randomIndex]);
          }
      };

      const removeWinner = (index: number) => {
          emit('remove-winner', index);
      };

      return {
          selectWinner,
          removeWinner
      };
  }
});
</script>
