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
      <SearchBar @filter-by-name="filterParticipants" />
      <ParticipantsTable :participants="filteredParticipants" />
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import RegistrationForm from './components/FormComponent.vue';
import WinnersList from './components/WinnersListComponent.vue';
import ParticipantsTable from './components/ParticipantsComponent.vue';
import SearchBar from './components/SearchBar.vue';

export default defineComponent({
  components: {
      RegistrationForm,
      WinnersList,
      SearchBar,
      ParticipantsTable
  },
  
  setup() {
      const winners = ref<{ name: string }[]>([]);
      const participants = ref<{ name: string; birthday: string; email: string; phone: string }[]>([]);
      const filteredParticipants = ref<{ name: string; birthday: string; email: string; phone: string }[]>(participants.value);

      const addParticipant = (participant: { name: string; birthday: string; email: string; phone: string }) => {
          participants.value.push(participant);
          filteredParticipants.value = participants.value; // Скидаємо фільтр після додавання
      };

      const filterParticipants = (filter: string) => {
          const regex = new RegExp(filter, 'i');
          filteredParticipants.value = participants.value.filter(participant =>
              regex.test(participant.name)
          );
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
          filteredParticipants,
          addParticipant,
          filterParticipants,
          addWinner,
          removeWinner
      };
      
  }
});
</script>
