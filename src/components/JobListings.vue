<script setup>

import JobListing from "./JobListing.vue";
import { RouterLink } from "vue-router";
import { reactive, onMounted } from 'vue';
import PulseLoader from 'vue-spinner/src/PulseLoader.vue';
import axios from "axios";

const props = defineProps({
    limit: {
        type: Number,
        default: 0
    },
    showButton: {
        type: Boolean,
        default: false
    }
})

let state = reactive({
    jobData: [],
    isLoading: true
});

// const jobListings = ref(jobData);



onMounted(async () => {
    try {
        const response = await axios.get("/api/jobs");
        state.jobData = response.data;
        if (props.limit) {
            state.jobData = state.jobData.slice(0, props.limit);
        }
    } catch (error) {
        console.error("Error fetching job data:", error);
    } finally {
        state.isLoading = false;
    }
})

</script>

<template>

    <section class="bg-green-50 px-4 py-10">
        <div class="container-xl lg:container m-auto">
            <h2 class="text-3xl font-bold text-green-500 mb-6 text-center">
                Browse Jobs
            </h2>

            <!---Loading Spinner -->
            <div v-if="state.isLoading" class="text-center text-gray-500 py-6">
                <PulseLoader />
            </div>

            <!--Show job listing after loading is done-->
            <div v-else class="grid grid-cols-1 md:grid-cols-3 gap-6">
                <JobListing v-for="job in state.jobData" :key="job.id" :id="job.id" :title="job.title" :type="job.type"
                    :location="job.location" :description="job.description" :salary="job.salary"
                    :company="job.company" />

            </div>
        </div>
    </section>

    <section v-if="showButton" class="m-auto max-w-lg my-10 px-6">
        <RouterLink to="/jobs" class="block bg-black text-white text-center py-4 px-6 rounded-xl hover:bg-gray-700">View
            All Jobs</RouterLink>
    </section>

</template>