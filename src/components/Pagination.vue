<template>
    <div class="flex h-16 w-full items-center justify-center bg-white">
        <button @click="prev" :disabled="currentPage === 1"
            class="flex items-center px-4 py-2 mx-1 text-gray-700 transition-colors duration-300 transform bg-white rounded-md dark:bg-gray-800 dark:text-gray-200 hover:bg-blue-600 dark:hover:bg-blue-500 hover:text-white dark:hover:text-gray-200 disabled:opacity-50 disabled:cursor-not-allowed">
            Anterior
        </button>
        <div class="flex items-center justify-between">
            <div
                class="items-center hidden px-4 py-2 mx-1 text-gray-700 transition-colors duration-300 transform bg-white rounded-md sm:flex">
                {{ currentPage }}</div> / <div
                class="items-center hidden px-4 py-2 mx-1 text-gray-700 transition-colors duration-300 transform bg-white rounded-md sm:flex">
                {{ totalPages }}</div>
        </div>
        <button @click="next" :disabled="currentPage === totalPages"
            class="flex items-center px-4 py-2 mx-1 text-gray-700 transition-colors duration-300 transform bg-white rounded-md dark:bg-gray-800 dark:text-gray-200 hover:bg-blue-600 dark:hover:bg-blue-500 hover:text-white dark:hover:text-gray-200 disabled:opacity-50 disabled:cursor-not-allowed">
            Siguiente
        </button>
    </div>
</template>

<script setup>
const props = defineProps(['currentPage', 'totalPages'])
const emit = defineEmits(['page-change'])

const prev = () => {
    if (props.currentPage > 1) emit('page-change', props.currentPage - 1)
}
const next = () => {
    if (props.currentPage < props.totalPages) emit('page-change', props.currentPage + 1)
}
</script>

<style>
.sticky {
    bottom: 0;
    left: 0;
    right: 0;
    z-index: 10;
}
</style>