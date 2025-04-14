<template>
    <form @submit.prevent="saveMovie" id="movieForm" method="POST" enctype="multipart/form-data">
        <div v-if="message" class="alert">
            {{ message }}
        </div>
        <div v-if="errors.length" class="alert alert-danger">
            <ul>
                <li v-for="error in errors" :key="error">
                    {{ error }}
                </li>
            </ul>
        </div>

        <div class="form-group">
            <label>Title</label>
            <input type="text" name="title" class="form-control" >
        </div>
        <div class="form_group">
            <label>Descriptions</label>
            <textarea name="description" class="form-control"></textarea>
        </div>
        <div class="form-group">
            <label>Poster</label>
            <input type="file" name="poster" class="form-control">
        </div>
        <button type="submit">Submit</button>
    </form>
</template>

<script setup>
import {ref, onMounted} from 'vue';

const csrf_token = ref('');
const message = ref('');
const errors = ref([]);

function getCsrfToken() {
    fetch('/api/v1/csrf-token')
    .then(res => res.json())
    .then(data => {csrf_token.value = data.csrf_token; 
        console.log(csrf_token.value);
    });
}

function saveMovie() {
    const movieForm = document.getElementById('movieForm');
    const formData = new FormData(movieForm);

    fetch('/api/v1/movies', {
        method: 'POST',
        body: formData,
        headers: {'X-CSRFToken': csrf_token.value}
    })
    .then(res => res.json())
    .then(data => {
        console.log(data)
        if (data.message){
            message.value = data.message;
            errors.value = [];
        }else {
            message.value = '';
            errors.value = Object.values(data.errors).flat();
        }
    })
    .catch(err => console.error(err));
}
onMounted(getCsrfToken());
</script>