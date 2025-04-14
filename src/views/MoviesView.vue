<template>
    <div class="movie-container">
        <div class="container"><h2 class="mb-4">Movies</h2></div>
        <div class="row">
            <div v-for="movie in movies" :key="movie.id" class="col-md-6 mb-3">
                <div class="card" style="max-width: 500px; margin: 1rem auto;">
                    <div class="row g-0">
                        <div class="col-md-4">
                            <img :src="movie.poster" alt="Movie Poster" class="img-fluid rounded-start" style="height: 250px; width:100%; object-fit: cover;">
                        </div>
                        <div class="col-md-8">
                            <div class="card-body">
                                <h5 class="card-title">{{ movie.title }}</h5>
                                <p class="card-text">{{ movie.description }}</p>
                            </div>
                        </div>    
                    </div>
                </div> 
            </div>
        </div>
    </div>
</template>

<script setup>
import {ref,onMounted} from 'vue';

let movies = ref([]);

function fetchMovies(){
    fetch('/api/v1/movies')
    .then(res => res.json())
    .then(data => {
        movies.value = data.movies;
    })
}
onMounted(fetchMovies);
</script>