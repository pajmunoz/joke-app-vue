<template>
    <div style="border: 1px solid #ccc; padding: 1rem; margin-bottom: 1rem;">
        <p><strong>{{ joke.setup }}</strong></p>
        <p>{{ joke.punchline }}</p>
        <small>Tipo: {{ joke.type }}</small>
    </div>
</template>

<script setup>
defineProps({
    joke: Object
})
</script>