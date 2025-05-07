<script setup>
import { ref, computed, onMounted, watch } from 'vue'
import JokeCard from '../components/JokeCard.vue'
import Pagination from '../components/Pagination.vue'
import AddJoke from '../components/AddJoke.vue'

const jokes = ref([])
const sortKey = ref('id')
const currentPage = ref(1)
const jokesPerPage = 5
const modaIsVisible = ref(false)

// ratings = { [id]: valor }
const ratings = ref({})

// Mensaje de toast
const toastMessage = ref('')

// Función para mostrar toast
const showToast = (msg) => {
    toastMessage.value = msg
    setTimeout(() => toastMessage.value = '', 3000)
}

const showModal = () => {
    modaIsVisible.value = true
}
const hideModal = () => {
    modaIsVisible.value = false
}

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
const addJoke = (joke) => {
    const id = Date.now()
    const newEntry = { id, ...joke }
    jokes.value.unshift(newEntry)

    // Guardar en localStorage
    const saved = JSON.parse(localStorage.getItem('custom_jokes')) || []
    saved.unshift(newEntry)
    localStorage.setItem('custom_jokes', JSON.stringify(saved))

    showToast('Chiste agregado 🎉')
}


const deleteJoke = (id) => {
    // confirmación
    if (!window.confirm('¿Estás seguro de que quieres eliminar este chiste?')) {
        return
    }
    jokes.value = jokes.value.filter(j => j.id !== id)

    // Eliminar rating si existe
    delete ratings.value[id]

    // Si era un chiste agregado por el usuario, también borrarlo del localStorage
    const customJokes = JSON.parse(localStorage.getItem('custom_jokes')) || []
    const updated = customJokes.filter(j => j.id !== id)
    localStorage.setItem('custom_jokes', JSON.stringify(updated))
    showToast('Chiste eliminado 🗑️')
}
</script>

<template>
    <div class="w-2xl mx-auto ">
        <h1>Chistes</h1>
        <div class="w-full my-4">
            <label>Ordenar por:</label>
            <select v-model="sortKey">
                <option value="id">ID</option>
                <option value="type">Tipo</option>
            </select>
        </div>

        <div v-if="toastMessage" class="fixed top-0 right-0 bg-gray-800 text-white px-4 py-2 rounded shadow">
            {{ toastMessage }}
        </div>
        <button @click="showModal"
            class="px-6 py-2 font-small cursor-pointer tracking-wide text-white capitalize transition-colors duration-300 transform bg-blue-600 rounded-lg hover:bg-blue-500 focus:outline-none focus:ring focus:ring-blue-300 focus:ring-opacity-80">
            Agregar chiste
        </button>
        <template v-if="modaIsVisible === true">
            <AddJoke @add="addJoke" @hideModal="hideModal" />
        </template>
        <template v-for="joke in paginatedJokes" :key="joke.id">
            <JokeCard :joke="joke" :rating="ratings[joke.id] || 0" @update-rating="setRating(joke.id, $event)"
                @delete-joke="deleteJoke" />
        </template>

        <Pagination :current-page="currentPage" :total-pages="totalPages" @page-change="goToPage" />
    </div>
</template>