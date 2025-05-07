<template>
    <div style="border: 1px solid #ccc; padding: 1rem; margin-bottom: 1rem;">
        <p><strong>{{ joke.setup }}</strong></p>
        <p>{{ joke.punchline }}</p>
        <small>Tipo: {{ joke.type }}</small>

        <div style="margin-top: 0.5rem;">
            <Rating v-model="localRating" />
            <p>Tu calificación: {{ localRating }}/5</p>
        </div>
    </div>
    <button @click="emit('delete-joke', joke.id)" style="margin-top: 1rem;">Eliminar</button>
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