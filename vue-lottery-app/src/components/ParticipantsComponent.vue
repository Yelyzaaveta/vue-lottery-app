<template>
  <table class="min-w-full bg-white shadow-md rounded">
      <thead>
          <tr>
              <th class="px-4 py-2">#</th>
              <th class="px-4 py-2">Name</th>
              <th class="px-4 py-2">Date of Birth</th>
              <th class="px-4 py-2">Email</th>
              <th class="px-4 py-2">Phone number</th>
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
</template>

<script lang="ts">
import { defineComponent} from 'vue';
import type {  PropType } from 'vue';

export default defineComponent({
  props: {
      participants: {
          type: Array as PropType<{ name: string; birthday: string; email: string; phone: string }[]>,
          required: true,
      },
  },
});
</script>
