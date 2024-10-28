<template>
    <form @submit.prevent="addParticipant" @keydown.enter="addParticipant" class="border border-gray p-6 bg-white shadow-md rounded mb-6">
        <h2 class="text-xl font-bold mb-4">REGISTRATION FORM</h2>
        <p class="text-sm text-gray-500 mb-4">Please fill in all the fields.</p>
  
        <div class="mb-4">
            <label class="block text-gray-700">Name</label>
            <input v-model="form.name" type="text" class="w-full p-2 border"
                :class="{ 'border-gray-400': !submitted, 'border-red-500': errors.name, 'border-green-500': submitted && !errors.name && form.name !== '' }"
                placeholder="Enter user name" />
            <p v-if="errors.name" class="text-red-500 text-sm">{{ errors.name }}</p>
        </div>
  
        <div class="mb-4">
            <label class="block text-gray-700">Date of Birth</label>
            <input v-model="form.birthday" type="date" class="w-full p-2 border"
                :class="{ 'border-gray-400': !submitted, 'border-red-500': errors.birthday, 'border-green-500': submitted && !errors.birthday && form.birthday !== '' }" />
            <p v-if="errors.birthday" class="text-red-500 text-sm">{{ errors.birthday }}</p>
        </div>
  
        <div class="mb-4">
            <label class="block text-gray-700">Email</label>
            <input v-model="form.email" type="text" class="w-full p-2 border"
                :class="{ 'border-gray-400': !submitted, 'border-red-500': errors.email, 'border-green-500': submitted && !errors.email && form.email !== '' }"
                placeholder="Enter email" />
            <p v-if="errors.email" class="text-red-500 text-sm">{{ errors.email }}</p>
        </div>
  
        <div class="mb-4">
            <label class="block text-gray-700">Phone number</label>
            <input v-model="form.phone" type="tel" class="w-full p-2 border"
                :class="{ 'border-gray-400': !submitted, 'border-red-500': errors.phone, 'border-green-500': submitted && !errors.phone && form.phone !== '' }"
                placeholder="Enter Phone number" />
            <p v-if="errors.phone" class="text-red-500 text-sm">{{ errors.phone }}</p>
        </div>
  
        <button type="submit" class="bg-blue-500 text-white px-4 py-2 rounded hover:bg-violet-500">Save</button>
    </form>
  </template>
  
  <script lang="ts">
  import { defineComponent, ref } from 'vue';
  import type { PropType } from 'vue';
  export interface Form {
      name: string;
      birthday: string;
      email: string;
      phone: string;
  }
  
  export default defineComponent({
    props: {
        participants: {
            type: Array as PropType<{ name: string; birthday: string; email: string; phone: string }[]>,
            required: true,
        },
        winners: {
            type: Array as PropType<{ name: string }[]>,
            required: true,
        },
    },
    setup(props, { emit }) {
        const form = ref<Form>({
            name: '',
            birthday: '',
            email: '',
            phone: ''
        });
        
        const errors = ref<{ [key: string]: string }>({
            name: '',
            birthday: '',
            email: '',
            phone: ''
        });
  
        const submitted = ref(false);
  
        const validateForm = () => {
            let isValid = true;
  
            if (!form.value.name) {
                errors.value.name = 'Name is required.';
                isValid = false;
            } else {
                errors.value.name = '';
            }
  
            if (!form.value.birthday) {
                errors.value.birthday = 'Date of Birth is required.';
                isValid = false;
            } else if (new Date(form.value.birthday) > new Date()) {
                errors.value.birthday = 'Date of Birth cannot be in the future.';
                isValid = false;
            } else {
                errors.value.birthday = '';
            }
  
            const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
           
            if (!form.value.email) {
                errors.value.email = 'Email is required.';
                isValid = false;
            } else if (!emailPattern.test(form.value.email)) {
                errors.value.email = 'Invalid email format.';
                isValid = false;
            } else if (props.participants.some(participant => participant.email === form.value.email)) {
                errors.value.email = 'This email is already exist';
                isValid = false;
            } else {
                errors.value.email = '';
            }
  
            const phonePattern = /^\+?3?8?(0\d{9})$/;
            if (!form.value.phone) {
                errors.value.phone = 'Phone number is required.';
                isValid = false;
            } else if (!phonePattern.test(form.value.phone)) {
                errors.value.phone = 'Invalid phone number format.';
                isValid = false;
            } else {
                errors.value.phone = '';
            }
  
            return isValid;
        };
  
        const addParticipant = () => {
            if (validateForm()) {
                emit('add-participant', { ...form.value });
                form.value = { name: '', birthday: '', email: '', phone: '' };
                submitted.value = false;
                errors.value = { name: '', birthday: '', email: '', phone: '' };
            } else {
                submitted.value = true;
            }
        };
  
        return {
            form,
            errors,
            submitted,
            addParticipant
        };
    }
  });
  </script>
  