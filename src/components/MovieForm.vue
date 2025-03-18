<template>
  <div class="container mt-5">
    <h2>Upload Movie</h2>
    <form @submit.prevent="saveMovie" id="movieForm">
      <div class="form-group mb-3">
        <label for="title" class="form-label">Movie Title</label>
        <input type="text" name="title" class="form-control" />
      </div>

      <div class="form-group mb-3">
        <label for="description" class="form-label">Description</label>
        <input type="text" name="description" class="form-control" />
      </div>

      <div class="form-group mb-3">
        <label for="poster" class="form-label">Poster Upload</label>
        <input type="file" name="poster" class="form-control" />
      </div>

      <button type="submit" class="btn btn-primary">Submit</button>
    </form>
  </div>
</template>

<script setup>
import { onMounted, ref } from 'vue';


let csrf_token = ref("");

const getCsrfToken = () => {
  fetch('/api/v1/csrf-token')
    .then((response) => response.json())
    .then((data) => {
      console.log(data);
      csrf_token.value = data.csrf_token;
    })
}

onMounted(() => {
  getCsrfToken();
})




const saveMovie = () => {

  let movieForm = document.getElementById('movieForm');
  console.log("this is movieform element: ", movieForm)
  let form_data = new FormData(movieForm);

  console.log("this is forma data: ", form_data)

  fetch("/api/v1/movies", {
    method: 'POST',
    body: form_data,
    headers: {
      'X-CSRFToken': csrf_token.value
    }

  })
    .then(function (response) {
      return response.json();
    })
    .then(function (data) {
      // display a success message 
      console.log(data);
      alert(`Success: ${data.message}`)
    })
    .catch(function (error) {
      console.log(error);
      alert("Error: ", error)
    });
} 
</script>

<style>
.container {
  max-width: 500px;
  margin-top: 50px;
}
</style>