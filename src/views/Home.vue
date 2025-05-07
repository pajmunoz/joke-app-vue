<template>
    <div>
        <h1>Chistes</h1>

        <label>Ordenar por:</label>
        <select v-model="sortKey">
            <option value="id">ID</option>
            <option value="type">Tipo</option>
        </select>

        <div>
            <JokeCard/>
        </div>

        <Pagination />
    </div>
</template>

<script setup>

</script>