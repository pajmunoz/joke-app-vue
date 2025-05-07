<template>
<div @click="handleClose" class="with-full h-full fixed inset-0 z-0 overflow-y-auto bg-gray-900 bg-opacity-50 transition-opacity ease-out duration-300 opacity-70"></div>
    <div class="relative flex justify-center">
        <div
            class="sm:fixed sm:w-1/2 relative w-full right-auto inline-block px-4 pt-5 pb-4 overflow-hidden text-left align-bottom transition-all transform bg-gray-100 rounded-lg shadow-xl rtl:text-right sm:my-8 sm:align-middle sm:max-w-sm sm:w-full sm:p-6">
            <div>


                <div class="mt-2">
                    <h3 class="text-lg text-center font-medium leading-6 text-gray-800 capitalize" id="modal-title">
                        Agrega un Chiste</h3>
                    <div class="mt-2 text-sm text-gray-500 ">
                        <form @submit.prevent="handleSubmit">
                            <div class="mb-4">
                                <label>Desarrollo: </label>
                                <input v-model="localJoke.setup" required />
                            </div>
                            <div class="mb-4">
                                <label>Chiste: </label>
                                <input v-model="localJoke.punchline" required />
                            </div>
                            <div class="mb-4">
                                <label>Tipo: </label>
                                <input v-model="localJoke.type" placeholder="general/programming/etc" required />
                            </div>
                            <div class="mt-5 sm:flex sm:items-center sm:justify-between">

                                <button @click="handleClose"
                                    class="w-full px-4 py-2 mt-2 cursor-pointer text-sm font-medium tracking-wide text-gray-700 capitalize transition-colors duration-300 transform border border-gray-500 rounded-md sm:mt-0 sm:w-auto sm:mx-2 dark:text-gray-800 dark:hover:bg-gray-800 hover:bg-gray-100 focus:outline-none focus:ring focus:ring-gray-600 focus:ring-opacity-20 hover:text-gray-50">
                                    Cerrar
                                </button>

                                <button type="submit"
                                    class="w-full px-4 py-2 mt-2 cursor-pointer text-sm font-medium tracking-wide text-white capitalize transition-colors duration-300 transform bg-blue-600 rounded-md sm:w-auto sm:mt-0 hover:bg-blue-500 focus:outline-none focus:ring focus:ring-blue-300 focus:ring-opacity-40">
                                    Agregar
                                </button>

                            </div>

                        </form>
                    </div>
                </div>
            </div>


        </div>

    </div>
</template>

<script setup>
import { ref } from 'vue'

const emit = defineEmits(['add', 'hideModal'])

const localJoke = ref({
    setup: '',
    punchline: '',
    type: ''
})

const handleSubmit = () => {
    emit('add', { ...localJoke.value })
    localJoke.value = { setup: '', punchline: '', type: '' }
}
const handleClose = () => {
    emit('hideModal')
}
</script>