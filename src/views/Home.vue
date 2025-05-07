<template>
    <div>
        <h1>Chistes</h1>

        <label>Ordenar por:</label>
        <select v-model="sortKey">
            <option value="id">ID</option>
            <option value="type">Tipo</option>
        </select>

        <div v-for="joke in paginatedJokes" :key="joke.id">
            <JokeCard :joke="joke" />
        </div>

        <Pagination :current-page="currentPage" :total-pages="totalPages" @page-change="goToPage" />
    </div>
</template>

<script setup>
import { ref, computed, onMounted } from 'vue'
import JokeCard from '../components/JokeCard.vue'
import Pagination from '../components/Pagination.vue'

const jokes = ref([])
const sortKey = ref('id')
const currentPage = ref(1)
const jokesPerPage = 5

const fetchJokes = async () => {
    const res = await fetch('http://localhost:3005/jokes/ten')
    const data = await res.json()
    jokes.value = data
}

onMounted(fetchJokes)

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
</script>