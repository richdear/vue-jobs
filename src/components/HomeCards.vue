<script setup>
import Card from "@/components/Card.vue";
import { RouterLink } from "vue-router";
</script>

<template>
    <section class="py-4">
        <div class="container-xl lg:container m-auto flex flex-row">

            <Card background="bg-gray-100" classes="basis-5/10 mx-1">

                <h2 class="text-2xl font-bold">For Developers</h2>
                <p class="mt-2 mb-4">
                    Browse our Vue jobs and start your career today
                </p>
                <RouterLink to="/jobs" class="inline-block bg-black text-white rounded-lg px-4 py-2 hover:bg-gray-700">
                    Browse Jobs
                </RouterLink>

            </Card>

           <Card background="bg-green-100"  classes="basis-5/10 mx-1">
                <h2 class="text-2xl font-bold">For Employers</h2>
                <p class="mt-2 mb-4">
                    List your job to find the perfect developer for the role
                </p>
                <RouterLink to="/jobs/add"
                    class="inline-block bg-green-500 text-white rounded-lg px-4 py-2 hover:bg-green-600">
                    Add Job
                </RouterLink>
            </Card>

        </div>
    </section>
</template>