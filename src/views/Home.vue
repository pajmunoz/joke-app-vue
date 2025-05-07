<script setup>
import { ref, computed, onMounted, watch } from 'vue'
import JokeCard from '../components/JokeCard.vue'
import Pagination from '../components/Pagination.vue'

const jokes = ref([])
const sortKey = ref('id')
const currentPage = ref(1)
const jokesPerPage = 5

// ratings = { [id]: valor }
const ratings = ref({})

// Cargar ratings desde localStorage
onMounted(() => {
    fetchJokes()
    const stored = localStorage.getItem('joke_ratings')
    if (stored) ratings.value = JSON.parse(stored)
})

watch(ratings, (newVal) => {
    localStorage.setItem('joke_ratings', JSON.stringify(newVal))
}, { deep: true })

const fetchJokes = async () => {
    const res = await fetch('https://official-joke-api.appspot.com/jokes/ten')
    const data = await res.json()
    jokes.value = data
}

const sortedJokes = computed(() =>
    [...jokes.value].sort((a, b) => {
        if (sortKey.value === 'id') return a.id - b.id
        return a.type.localeCompare(b.type)
    })
)

const totalPages = computed(() =>
    Math.ceil(sortedJokes.value.length / jokesPerPage)
)

const paginatedJokes = computed(() => {
    const start = (currentPage.value - 1) * jokesPerPage
    return sortedJokes.value.slice(start, start + jokesPerPage)
})

const goToPage = (page) => {
    currentPage.value = page
}

const setRating = (id, value) => {
    ratings.value[id] = value
}
</script>

<template>
    <div>
        <h1>Chistes</h1>

        <label>Ordenar por:</label>
        <select v-model="sortKey">
            <option value="id">ID</option>
            <option value="type">Tipo</option>
        </select>

        <div v-for="joke in paginatedJokes" :key="joke.id">
            <JokeCard :joke="joke" :rating="ratings[joke.id] || 0" @update-rating="setRating(joke.id, $event)" />
        </div>

        <Pagination :current-page="currentPage" :total-pages="totalPages" @page-change="goToPage" />
    </div>
</template>