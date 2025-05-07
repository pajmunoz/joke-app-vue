<template>
    <div v-if="show" class="flex items-center justify-center fixed top-50 left-0 w-full">
        

        <div class="w-auto bg-white rounded-lg shadow-lg modal-content sm:w-100 z-10">
            <p>{{ message }}</p>
            <div class="flex items-center justify-between mt-5 flex-col sm:flex-row sm:items-center sm:justify-between">
                <button
                    class="w-full sm:w-1/2 px-4 py-2 mt-2 cursor-pointer text-sm font-medium tracking-wide text-white capitalize transition-colors duration-300 transform bg-blue-600 rounded-md  sm:mt-0 hover:bg-blue-500 focus:outline-none focus:ring focus:ring-blue-300 focus:ring-opacity-40"
                    @click="$emit('confirm')">Sí</button>
                <button
                    class="w-full sm:w-1/2 px-4 py-2 mt-2 cursor-pointer text-sm font-medium tracking-wide text-gray-700 capitalize transition-colors duration-300 transform border border-gray-500 rounded-md sm:mt-0 sm:mx-2 dark:text-gray-800 dark:hover:bg-gray-800 hover:bg-gray-100 focus:outline-none focus:ring focus:ring-gray-600 focus:ring-opacity-20 hover:text-gray-50"
                    @click="$emit('cancel')">Cancelar</button>
            </div>
        </div>
        <div @click="$emit('cancel')"
            class="with-full h-full fixed inset-0 z-0 overflow-y-auto bg-gray-900 bg-opacity-50 transition-opacity ease-out duration-300 opacity-70">
        </div>
    </div>
</template>

<script setup>
defineProps({
    show: Boolean,
    message: {
        type: String,
        default: '¿Estás seguro?'
    }
})
defineEmits(['confirm', 'cancel'])
</script>

<style scoped>
.modal-content {
    background: white;
    padding: 2rem;
    border-radius: 8px;
    text-align: center;
}
</style>