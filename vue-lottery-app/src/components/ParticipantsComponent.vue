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
        <td class="border px-4 py-2">
          <button @click="openEditModal(participant)" class="text-blue-500">_/</button>
          <button @click="openDeleteModal(participant)" class="text-red-500 ml-4">X</button>
        </td>
      </tr>
    </tbody>
  </table>

  <Modal v-if="showEditModal" :isOpen="showEditModal" title="Edit Participant Data" confirmButtonText="Save"
    @close="closeEditModal" @confirm="updateParticipant">
    <form>
      <input v-model="editedParticipant.name" placeholder="Name" class="w-full p-2 border mb-2" />
      <input v-model="editedParticipant.birthday" type="date" placeholder="Date of Birth"
        class="w-full p-2 border mb-2" />
      <input v-model="editedParticipant.email" placeholder="Email" class="w-full p-2 border mb-2" />
      <input v-model="editedParticipant.phone" placeholder="Phone number" class="w-full p-2 border mb-2" />
    </form>
  </Modal>

  
  <Modal v-if="showDeleteModal" :isOpen="showDeleteModal" title="Confirm Delete" confirmButtonText="Delete"
    confirmButtonClass="bg-red-500 text-white px-4 py-2 rounded hover:bg-red-600" @close="closeDeleteModal"
    @confirm="confirmDeleteParticipant">
    <p>Are you sure you want to delete {{ deletedParticipant?.name }} ?</p>
  </Modal>
</template>

<script lang="ts">
import { defineComponent, onMounted, onUnmounted, ref } from 'vue';
import Modal from './ModalComponent.vue';

export default defineComponent({
  components: { Modal },
  props: {
    participants: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      showEditModal: false,
      showDeleteModal: false,
      editedParticipant: null as { name: string; birthday: string; email: string; phone: string } | null,
      deletedParticipant: null as { name: string; email: string } | null,
    };
  },
  methods: {
    openEditModal(participant) {
      this.editedParticipant = { ...participant };
      this.showEditModal = true;
    },
    closeEditModal() {
      this.showEditModal = false;
    },
    updateParticipant() {
      const index = this.participants.findIndex(p => p.email === this.editedParticipant?.email);
      if (index !== -1) this.participants.splice(index, 1, this.editedParticipant);
      this.closeEditModal();
    },
    openDeleteModal(participant) {
      this.deletedParticipant = participant;
      this.showDeleteModal = true;
    },
    closeDeleteModal() {
      this.showDeleteModal = false;
    },
    confirmDeleteParticipant() {
      const index = this.participants.findIndex(p => p.email === this.deletedParticipant?.email);
      if (index !== -1) this.participants.splice(index, 1);
      this.closeDeleteModal();
    },
    handleKeyDown(event: KeyboardEvent) {
      if (event.key === 'Escape') {
        if (this.showEditModal) this.closeEditModal();
        if (this.showDeleteModal) this.closeDeleteModal();
      }
    },
  },
  mounted() {
    window.addEventListener('keydown', this.handleKeyDown);
  },
  unmounted() {
    window.removeEventListener('keydown', this.handleKeyDown);
  },
});
</script>
