<script setup>
import { defineProps } from 'vue';

defineProps({
    background: {
        type: String,
        default: 'bg-gray-100'
    },
    classes:{
        type: String,
        default:""
    }
});
</script>
<template>
    <div :class="`${background} p-6 rounded-lg shadow-md ${classes}`">
        <slot></slot>
    </div>

</template>