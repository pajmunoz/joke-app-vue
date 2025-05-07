<template>
    <div class="  px-8 py-4 my-4 bg-white rounded-lg shadow-md ">
        <div class="flex items-center justify-between">
            <span class="text-sm font-light text-gray-600">Tipo: {{ joke.type }}</span>
            <button
                class="px-3 py-1 text-sm font-bold text-gray-600 transition-colors duration-300 transform bg-gray-100 rounded cursor-pointer hover:bg-gray-200"
                @click="emit('delete-joke', joke.id)">Eliminar</button>
        </div>

        <div class="mt-2">
            <p class="text-xl font-bold text-gray-700  hover:text-gray-600  hover:underline">{{ joke.setup }}</p>
            <p class="mt-2 text-gray-600 ">{{ joke.punchline }}</p>
        </div>

        <div class="flex items-center justify-between mt-4">
            <Rating v-model="localRating" />
            <p class="font-bold text-gray-700 cursor-pointer" tabindex="0" role="link">Tu calificación: {{
                localRating }}/5</p>
        </div>

    </div>

</template>

<script setup>

import Rating from './Rating.vue'
import { ref, watch } from 'vue'

const props = defineProps({
    joke: Object,
    rating: Number
})

const emit = defineEmits(['update-rating', 'delete-joke'])
const localRating = ref(props.rating)

watch(localRating, (value) => {
    emit('update-rating', value)
})
watch(() => props.rating, (value) => {
    localRating.value = value
})
</script>