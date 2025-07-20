<script setup>
import { reactive, onMounted } from 'vue';
import PulseLoader from 'vue-spinner/src/PulseLoader.vue';
import axios from 'axios';
import { useRoute, RouterLink } from 'vue-router';
import BackButton from '@/components/BackButton.vue';
import router from "@/router"
import { useToast } from 'vue-toastification';

const route = useRoute();

const state = reactive({
    id: route.params.id,
    type: '',
    title: '',
    location: '',
    description: '',
    salary: '',
    company: {},
    isLoading: true
})
const toast = useToast();

onMounted(async () => {
    try {
        const response = await axios.get(`/api/jobs/${state.id}`);
        console.log(response.data);
        state.type = response.data.type;
        state.title = response.data.title;
        state.location = response.data.location;
        state.description = response.data.description;
        state.salary = response.data.salary;
        state.company = response.data.company;
    } catch (error) {
        console.error("Error fetching job data:", error);
    } finally {
        state.isLoading = false;
    }

})

const handleDelete = async () => {
    try {
        const confirm = window.confirm(`Are you sure you want to delete job ${state.title} ?`);
        if (confirm) {
            const response = await axios.delete(`/api/jobs/${state.id}`);
            console.log(response.data);
            toast.success(`Job was deleted successfully.`);
            router.push(`/jobs/`);
        }

    } catch (error) {
        console.error("Error fetching job data:", error);
        toast.success(`Error deleting job!`);
    } finally {
        state.isLoading = false;
    }
}

</script>

<template>
    <BackButton />
    <div v-if="state.isLoading" class="text-center text-gray-500 py-6">
        <PulseLoader />
    </div>

    <section v-else class="bg-green-50">
        <div class="container m-auto py-10 px-6">
            <div class="flex flex-row ">
                <main class="basis-7/10 mr-2">
                    <div class="bg-white p-6 rounded-lg shadow-md text-center md:text-left">
                        <div class="text-gray-500 mb-4">{{ state.type }}</div>
                        <h1 class="text-3xl font-bold mb-4">{{ state.title }}</h1>
                        <div class="text-gray-500 mb-4 flex align-middle justify-center md:justify-start">
                            <i class="pi pi-map-marker text-orange-700 mr-2"></i>
                            <p class="text-orange-700">{{ state.location }}</p>
                        </div>
                    </div>

                    <div class="bg-white p-6 rounded-lg shadow-md mt-6">
                        <h3 class="text-green-800 text-lg font-bold mb-6">
                            Job Description
                        </h3>

                        <p class="mb-4">
                            {{ state.description }}
                        </p>

                        <h3 class="text-green-800 text-lg font-bold mb-2">Salary</h3>

                        <p class="mb-4">{{ state.salary }}</p>
                    </div>
                </main>

                <!-- Sidebar -->
                <aside class="basis-3/10">
                    <!-- Company Info -->
                    <div class="bg-white p-6 rounded-lg shadow-md">
                        <h3 class="text-xl font-bold mb-6">Company Info</h3>

                        <h2 class="text-2xl">{{ state.company.name }}</h2>

                        <p class="my-2">
                            {{ state.company.description }} </p>

                        <hr class="my-4" />

                        <h3 class="text-xl">Contact Email:</h3>

                        <p class="my-2 bg-green-100 p-2 font-bold">
                            {{ state.company.contactEmail }}
                        </p>

                        <h3 class="text-xl">Contact Phone:</h3>

                        <p class="my-2 bg-green-100 p-2 font-bold">
                            {{ state.company.contactPhone }}</p>
                    </div>

                    <!-- Manage -->
                    <div class="bg-white p-6 rounded-lg shadow-md mt-6">
                        <h3 class="text-xl font-bold mb-6">Manage Job</h3>
                        <RouterLink :to="`/jobs/edit/${state.id}`"
                            class="bg-green-500 hover:bg-green-600 text-white text-center font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline mt-4 block">
                            Edit
                            Job</RouterLink>
                        <button @click="handleDelete"
                            class="bg-red-500 hover:bg-red-600 text-white font-bold py-2 px-4 rounded-full w-full focus:outline-none focus:shadow-outline mt-4 block">
                            Delete Job
                        </button>
                    </div>
                </aside>
            </div>
        </div>
    </section>

</template>