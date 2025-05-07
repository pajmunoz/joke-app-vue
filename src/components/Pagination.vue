<template>
    <div style="margin-top: 1rem;">
        <button @click="prev" :disabled="currentPage === 1">Anterior</button>
        <span>Página {{ currentPage }} de {{ totalPages }}</span>
        <button @click="next" :disabled="currentPage === totalPages">Siguiente</button>
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