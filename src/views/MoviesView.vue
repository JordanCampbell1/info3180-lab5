<script setup>
import { ref, onMounted } from 'vue';

let movies = ref([]);

const fetchMovies = () => {
    fetch("/api/v1/movies", {
        method: 'GET'
    })
        .then((response) => response.json())
        .then((data) => {
            movies.value = data.movies;
        })
        .catch((error) => {
            console.error(error);
        });
};

onMounted(() => {
    fetchMovies();
});
</script>

<template>
    <div class="movies-container">
        <h1>Movies</h1>
        <div class="movies-grid">
            <div v-for="movie in movies" :key="movie.id" class="movie-card">
                <img :src="movie.poster" :alt="movie.title" class="movie-poster" />
                <div class="movie-info">
                    <h2 class="movie-title">{{ movie.title }}</h2>
                    <p class="movie-description">{{ movie.description }}</p>
                </div>
            </div>
        </div>
     
    </div>
</template>

<style scoped>
.movies-container {
    text-align: left;
    padding: 20px;
}

.movies-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(500px, 1fr));
    gap: 20px;
    padding: 20px;
}

.movie-card {
    display: flex;
    align-items: center;
    background: #fff;
    border-radius: 10px;
    padding: 15px;
    box-shadow: 0px 4px 6px rgba(0, 0, 0, 0.1);
    transition: transform 0.2s;
    max-width: 600px;
    width: 100%;
}

.movie-card:hover {
    transform: scale(1.02);
}

.movie-poster {
    width: 120px;
    height: auto;
    border-radius: 10px;
    margin-right: 15px;
}

.movie-info {
    text-align: left;
    flex: 1;
}

.movie-title {
    font-size: 18px;
    font-weight: bold;
    margin-bottom: 5px;
}

.movie-description {
    font-size: 14px;
    color: #555;
}

</style>
