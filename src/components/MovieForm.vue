<template>
  <!-- Success Message -->
  <div v-if="successMessage" class="alert alert-success mt-3">
    {{ successMessage }}
  </div>

  <!-- Error Message -->
  <div v-if="errorMessage" class="alert alert-danger mt-3">
    {{ errorMessage }}
  </div>

  <div class="container mt-5">
    <h2>Upload Movie</h2>
    <form @submit.prevent="saveMovie" id="movieForm">
      <div class="form-group mb-3">
        <label for="title" class="form-label">Movie Title</label>
        <input type="text" name="title" class="form-control" />
      </div>

      <div class="form-group mb-3">
        <label for="description" class="form-label">Description</label>
        <textarea name="description" class="form-control" rows="4"></textarea>

        <!-- <input type="text" name="description" class="form-control" /> -->
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
let successMessage = ref("");  // For success feedback
let errorMessage = ref("");    // For error feedback

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
      // alert(`Success: ${data.message}`)
      // On success, display the success message
      successMessage.value = `Success: ${data.message}`;
      errorMessage.value = "";  // Clear any previous error message
    })
    .catch(function (error) {
      console.log(error);
      // alert("Error: ", error)
      // On error, display the error message
      successMessage.value = "";  // Clear any previous success message
      errorMessage.value = `Error: ${error.message || 'An unexpected error occurred'}`;
    });
} 
</script>

<style>
.container {
  max-width: 500px;
  margin-top: 50px;
}

.alert {
  font-weight: bold;
}

.alert-success {
  background-color: #d4edda;
  color: #155724;
}

.alert-danger {
  background-color: #f8d7da;
  color: #721c24;
}

textarea{
  resize: none;
}
</style>