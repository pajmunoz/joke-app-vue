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
    const res = await fetch('http://localhost:3005/jokes/ten')
    const data = await res.json()
    jokes.value = [...(JSON.parse(localStorage.getItem('custom_jokes')) || []), ...data]
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

const newJoke = ref({
    setup: '',
    punchline: '',
    type: ''
})

const addJoke = () => {
    const id = Date.now()
    const newEntry = { id, ...newJoke.value }
    jokes.value.unshift(newEntry)

    // guardar en localStorage
    const saved = JSON.parse(localStorage.getItem('custom_jokes')) || []
    saved.unshift(newEntry)
    localStorage.setItem('custom_jokes', JSON.stringify(saved))

    newJoke.value = { setup: '', punchline: '', type: '' }
}

const deleteJoke = (id) => {
    jokes.value = jokes.value.filter(j => j.id !== id)

    // Eliminar rating si existe
    delete ratings.value[id]

    // Si era un chiste agregado por el usuario, también borrarlo del localStorage
    const customJokes = JSON.parse(localStorage.getItem('custom_jokes')) || []
    const updated = customJokes.filter(j => j.id !== id)
    localStorage.setItem('custom_jokes', JSON.stringify(updated))
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

        <h2>Agregar un nuevo chiste</h2>
        <form @submit.prevent="addJoke" style="margin-bottom: 2rem;">
            <div>
                <label>Setup:</label>
                <input v-model="newJoke.setup" required />
            </div>
            <div>
                <label>Punchline:</label>
                <input v-model="newJoke.punchline" required />
            </div>
            <div>
                <label>Tipo:</label>
                <input v-model="newJoke.type" placeholder="general/programming/etc" required />
            </div>
            <button type="submit">Agregar</button>
        </form>

        <div v-for="joke in paginatedJokes" :key="joke.id">
            <JokeCard :joke="joke" :rating="ratings[joke.id] || 0" @update-rating="setRating(joke.id, $event)"
                @delete-joke="deleteJoke" />
        </div>

        <Pagination :current-page="currentPage" :total-pages="totalPages" @page-change="goToPage" />
    </div>
</template>