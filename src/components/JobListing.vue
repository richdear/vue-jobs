<script setup>
import {ref, computed} from 'vue'
import { RouterLink } from 'vue-router';

const props = defineProps({
    id: {
        type: [String, Number],
        required: true
    },
    title: {
        type: String,
        default: "Senior Vue Developer"
    },
    type: {
        type: String,
        default: "We are seeking a talented Front-End Developer to join our team in Boston, MA."
    },
    location: {
        type: String,
        default: "$70 - $80K / Year"
    },
    description: {
        type: String,
        default: "Boston, MA"
    },
    salary: {
        type: String,
        default: "We are seeking a talented Front-End Developer to join our team in Boston, MA."
    },
    company: {
        type: String,
        default: "$70 - $80K / Year"
    }
});

const showFullDescription = ref(false);

const truncateDescription = computed(() => {
    let description = props.description;
    if (!showFullDescription.value) {
        description = description.substring(0, 90) + "...";
    }
    return description;
})

const toggleFullDescription = () => {
    showFullDescription.value = !showFullDescription.value;
}

</script>

<template>
    <div class="bg-white rounded-xl shadow-md relative">
        <div class="p-4">
            <div class="mb-6">
                <div class="text-gray-600 my-2">{{ title }}</div>
                <h3 class="text-xl font-bold">{{ type }}</h3>
            </div>

            <div class="mb-5">
             <div>{{ truncateDescription }}</div>
             <button class="text-green-500 hover:text-green-600 mb-5" @click="toggleFullDescription">
                {{ showFullDescription ?'Less': 'More' }}
             </button>   
            </div>

            <h3 class="text-green-500 mb-2">{{ salary }}</h3>

            <div class="border border-gray-100 mb-5"></div>

            <div class="flex flex-col lg:flex-row justify-between mb-4">
                <div class="text-orange-700 mb-3">
                    <i class="pi pi-map-marker text-orange-700"></i>
                    {{ location }}
                </div>
                <RouterLink :to="`/jobs/${id}`"
                    class="h-[36px] bg-green-500 hover:bg-green-600 text-white px-4 py-2 rounded-lg text-center text-sm">
                    Read More
            </RouterLink>
            </div>
        </div>
    </div>
</template>