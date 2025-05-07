<template>
    <div style="border: 1px solid #ccc; padding: 1rem; margin-bottom: 1rem;">
        <p><strong>{{ joke.setup }}</strong></p>
        <p>{{ joke.punchline }}</p>
        <small>Tipo: {{ joke.type }}</small>

        <div style="margin-top: 0.5rem;">
            <Rating v-model="rating" />
            <p>Tu calificación: {{ rating }}/5</p>
        </div>
    </div>
</template>

<script setup>

import Rating from './Rating.vue'
import { ref } from 'vue'

defineProps({
    joke: Object
})

const rating = ref(0)
</script>