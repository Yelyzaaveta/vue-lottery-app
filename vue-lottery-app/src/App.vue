<template>
  <div class="max-w-3xl mx-auto mt-[60px]">
      <WinnersList 
          :winners="winners"
          :participants="participants"
          @select-winner="addWinner"
          @remove-winner="removeWinner"
      />
      <RegistrationForm 
            :participants="participants"
            :winners="winners" 
            @add-participant="addParticipant"
        />
      <ParticipantsTable :participants="participants" />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import RegistrationForm from './components/FormComponent.vue';
import WinnersList from './components/WinnersListComponent.vue';
import ParticipantsTable from './components/ParticipantsComponent.vue';

export default defineComponent({
  components: {
      RegistrationForm,
      WinnersList,
      ParticipantsTable
  },
  setup() {
      const winners = ref<{ name: string }[]>([]);
      const participants = ref<{ name: string; birthday: string; email: string; phone: string }[]>([]);

      const addParticipant = (participant: { name: string; birthday: string; email: string; phone: string }) => {
          participants.value.push(participant);
      };

      const addWinner = (winner: { name: string }) => {
          winners.value.push(winner);
      };

      const removeWinner = (index: number) => {
          winners.value.splice(index, 1);
      };

      return {
          winners,
          participants,
          addParticipant,
          addWinner,
          removeWinner
      };
  }
});
</script>
