<template>
  
  <table class="min-w-full bg-white shadow-md rounded">
    <thead>
     

      <tr>
        <th class="px-4 py-2">#</th>
        <th class="px-4 py-2">Name</th>
        <th class="px-4 py-2">Date of Birth</th>
        <th class="px-4 py-2">Email</th>
        <th class="px-4 py-2">Phone number</th>
        <th class="px-4 py-2">Actions</th>
      </tr>
    </thead>
    <tbody>
      <tr v-for="(participant, index) in participants" :key="index">
        <td class="border px-4 py-2">{{ index + 1 }}</td>
        <td class="border px-4 py-2">{{ participant.name }}</td>
        <td class="border px-4 py-2">{{ participant.birthday }}</td>
        <td class="border px-4 py-2">{{ participant.email }}</td>
        <td class="border px-4 py-2">{{ participant.phone }}</td>
        
      
      </tr>
    </tbody>
    
  </table>
  <div class="w-[600px] flex flex-row">
        <div class="px-4 py-2 flex flex-row">
          <p>Sort By Name</p>
        <button @click="sortByName" class="ml-2">A-Z</button>
      </div>
      <div class="px-4 py-2 flex flex-row">
        <p>Date of Birth</p>
        <button @click="sortByBirthday" class="ml-2">/\</button>
        <button @click="sortByBirthdayDown" class="ml-2">\/</button>
      </div>
      </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import type { PropType } from 'vue';

export default defineComponent({
  props: {
    participants: {
      type: Array as PropType<{ name: string; birthday: string; email: string; phone: string }[]>,
      required: true,
    },
  },
  methods: {
    sortByName() {
        this.participants.sort((a, b) => a.name.localeCompare(b.name));
    },
    sortByBirthday() {
        this.participants.sort((a, b) => new Date(a.birthday).getTime() - new Date(b.birthday).getTime());
    },
    sortByBirthdayDown() {
        this.participants.sort((a, b) => new Date(b.birthday).getTime() - new Date(a.birthday).getTime());
    },
    
}

});
</script>
