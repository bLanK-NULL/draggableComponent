<template>
    <svg>
        <line x1="0" y1="0" x2="0" y2="0" stroke-width="1"></line>
    </svg>
</template>

<script setup lang="ts">
const props = defineProps({
    position: {
        type: Object,
        default: () => { }
    },
    style: {
        type: Object,
        default: () => { }
    }
})
</script>

<style scoped></style>