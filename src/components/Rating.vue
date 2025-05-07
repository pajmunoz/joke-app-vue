<template>
    <div>
        <span v-for="star in 5" :key="star" @click="setRating(star)" @mouseover="hoverRating = star"
            @mouseleave="hoverRating = 0" style="cursor: pointer; font-size: 1.5rem;">
            {{ star <= (hoverRating || modelValue) ? '⭐' : '☆' }} </span>
    </div>
</template>

<script setup>

import { ref } from 'vue'
const props = defineProps({
    modelValue: {
        type: Number,
        default: 0
    }
})

const emit = defineEmits(['update:modelValue'])
const hoverRating = ref(0)

const setRating = (value) => {
    emit('update:modelValue', value)
}
</script>