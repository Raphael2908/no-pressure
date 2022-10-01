<template>
    <AppLayout title="Dashboard">
        <template #header>
            <h2 class="font-semibold text-xl text-gray-800 leading-tight">
                Pins
            </h2>
        </template>

        <div class="py-12">
            <div class="max-w-7xl mx-auto sm:px-6 lg:px-8">
                <h1 class="text-xl">Create your pins</h1>
                <div class="relative">
                    <button 
                    @click="replicate()" 
                    class="absolute right-0 top-0 text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 mr-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800">
                        Add
                    </button>
                    <button 
                    @click="" 
                    class="absolute right-0 top-12 text-white bg-red-700 hover:bg-red-800 focus:ring-4 focus:ring-red-300 font-medium rounded-lg text-sm px-5 py-2.5 mr-2 mb-2 dark:bg-red-600 dark:hover:bg-red-700 focus:outline-none dark:focus:ring-red-800">
                        Delete
                    </button>
                </div>
                <div id="pinForm" class="">
                    <form @submit.prevent="submit">
                        <input type="hidden" name="user_id" id="user_id" :value="user.id">
                        <div v-for="(pin, ind) in pins">
                            <div class="mt-4 block w-5/6">
                                <input v-model="form.title" :key="ind" type="text" name="title" id="title" class="w-2/5 border-gray-300 focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50 rounded-md shadow-sm" placeholder="Title">
                                <div class="text-red-700" v-if="errors.title">{{ errors.title }}</div>
                            </div>
                            <div class="mt-4 block w-5/6">
                                <textarea v-model="form.description" :key="ind" name="description" id="description" class="w-3/4 h-32 border-gray-300 focus:border-indigo-300 focus:ring focus:ring-indigo-200 focus:ring-opacity-50 rounded-md shadow-sm" placeholder="Description"></textarea>
                                <div class="text-red-700" v-if="errors.description">{{ errors.description }}</div>
                            </div>
                            <div class="mt-4">
                                <button :key="ind" :class="{ 'opacity-25': form.processing }" :disabled="form.processing" type="submit" class="right-0 top-0 text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:ring-blue-300 font-medium rounded-lg text-sm px-5 py-2.5 mr-2 mb-2 dark:bg-blue-600 dark:hover:bg-blue-700 focus:outline-none dark:focus:ring-blue-800">Save</button>
                            </div>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </AppLayout>
</template>

<script>
import AppLayout from '@/Layouts/AppLayout.vue';
import { useForm } from '@inertiajs/inertia-vue3';

export default {
    setup() {
        const form = useForm({
            title: "",
            description: "",
        });

        return { form, }
    },
    data() {
        return {pins: [], }
    },
    methods: {
        submit() {
            console.log(this.pins);
            this.form.transform((data) => ({
                ...data,
                user_id: data.user_id ? "" : this.user_id,
            })).post("/pins/store", this.form.values);
        },
        replicate() {
            this.pins.push({ value: "" });
        },
    },
    mounted() {
        this.user_id = document.getElementById("user_id").value;
    },
    components: {
        AppLayout,
    },
    props: { errors: Object, user: Object },
}
</script>