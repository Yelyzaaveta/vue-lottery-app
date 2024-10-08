<template>
    <div class="max-w-3xl mx-auto mt-[60px]">

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

        <form @submit.prevent="addParticipant" class="border border-gray p-6 bg-white shadow-md rounded mb-6">
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
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
    data() {
        return {
            winners: [] as { name: string }[],
            participants: [] as { name: string; birthday: string; email: string; phone: string }[],
            form: {
                name: '',
                birthday: '',
                email: '',
                phone: ''
            },
            errors: {
                name: '',
                birthday: '',
                email: '',
                phone: ''
            },
            submitted: false,
        };
    },
    methods: {
        validateForm() {
            let isValid = true;

            if (!this.form.name) {
                this.errors.name = 'Name is required.';
                isValid = false;
            } else {
                this.errors.name = '';
            }

            if (!this.form.birthday) {
                this.errors.birthday = 'Date of Birth is required.';
                isValid = false;
            } else if (new Date(this.form.birthday) > new Date()) {
                this.errors.birthday = 'Date of Birth cannot be in the future.';
                isValid = false;
            } else {
                this.errors.birthday = '';
            }

            const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            if (!this.form.email) {
                this.errors.email = 'Email is required.';
                isValid = false;
            } else if (!emailPattern.test(this.form.email)) {
                this.errors.email = 'Invalid email format.';
                isValid = false;
            } else {
                this.errors.email = '';
            }

            const phonePattern = /^\+?3?8?(0\d{9})$/;
            if (!this.form.phone) {
                this.errors.phone = 'Phone number is required.';
                isValid = false;
            } else if (!phonePattern.test(this.form.phone)) {
                this.errors.phone = 'Invalid phone number format.';
                isValid = false;
            } else {
                this.errors.phone = '';
            }

            return isValid;
        },

        addParticipant() {
            if (this.validateForm()) {
                this.participants.push({ ...this.form });
                this.form.name = '';
                this.form.birthday = '';
                this.form.email = '';
                this.form.phone = '';
                this.errors.name = '';
                this.errors.birthday = '';
                this.errors.email = '';
                this.errors.phone = '';
                this.submitted = false;
            } else {
                this.submitted = true;
            }
        },


        selectWinner() {
            if (this.participants.length > 0 && this.winners.length < 3) {
                const randomIndex = Math.floor(Math.random() * this.participants.length);
                this.winners.push(this.participants[randomIndex]);
            }
        },

        removeWinner(index: number) {
            this.winners.splice(index, 1);
        }
    }
});
</script>
