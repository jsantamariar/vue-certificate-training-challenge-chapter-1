<template>
  <div class="flex justify-between px-20 py-4 mb-4">
    <div class="flex gap-4">
      <p class="text-white">Total Movies: {{ moviesCount }}</p>
      <span class="text-white">/</span>
      <p v-if="moviesRating" class="text-white">
        Average Rating: {{ moviesRating.toFixed(1) }}
      </p>
      <p class="text-white" v-else>Average Rating: 0</p>
    </div>
    <div class="flex gap-4">
      <button
        :disabled="!moviesCount"
        @click="handleRemoveRatings"
        :class="[
          'text-white bg-blue-400 p-3 rounded',
          (moviesCount < 1 || (!moviesRating && moviesCount > 0)) &&
            'bg-gray-400 cursor-not-allowed',
        ]"
      >
        Remove ratings
      </button>
      <button
        @click="showAddNewMovieForm"
        class="text-white bg-blue-400 p-3 rounded"
      >
        Add movie
      </button>
    </div>
  </div>
  <CustomCards :movies="movies" @edit-movie="openEditMovie" />
  <div
    v-if="isAddNewFormVisible"
    class="fixed inset-0 z-20 bg-black bg-opacity-50 flex justify-center items-center backdrop-blur-sm"
  >
    <AddNewForm
      :movieToEdit="movieToEdit"
      @close="closeForm"
      @add-movie="handleAddMovie"
      @edit-movie="handleEditMovie"
    />
  </div>
</template>

<script setup>
  import { reactive, ref, computed } from "vue";
  import CustomCards from "./components/CustomCards.vue";
  import AddNewForm from "./components/AddNewForm.vue";
  import { items } from "./movies.json";

  const isAddNewFormVisible = ref(false);
  const movies = reactive(items);
  const movieToEdit = ref(null);

  const moviesCount = computed(() => movies.length);
  const moviesRating = computed(() => {
    const totalRating = movies.reduce((acc, movie) => acc + movie.rating, 0);
    return totalRating / moviesCount.value;
  });

  const showAddNewMovieForm = () => {
    movieToEdit.value = null;
    isAddNewFormVisible.value = true;
  };

  const handleAddMovie = (movie) => {
    movies.push(movie);
  };

  const handleRemoveRatings = () => {
    movies.forEach((movie) => (movie.rating = 0));
  };

  const openEditMovie = (movie) => {
    isAddNewFormVisible.value = true;
    movieToEdit.value = { ...movie };
  };

  const handleEditMovie = (movieEdited) => {
    const movieIndex = movies.findIndex((m) => m.id === movieEdited.id);
    if (movieIndex !== -1) {
      movies[movieIndex] = movieEdited;
    }
  };

  const closeForm = () => {
    movieToEdit.value = null;
    isAddNewFormVisible.value = false;
  };
</script>
