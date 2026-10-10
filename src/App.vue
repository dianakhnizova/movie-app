<script setup>
import MovieCard from "./components/MovieCard.vue";
import SelectedMovieForm from "./components/SelectedMovieForm.vue";
import { items } from "./movies.json";
import { ref, computed } from "vue";

const movieList = ref(items);
const defaultMovieState = {
  id: null,
  name: "",
  description: "",
  genres: [],
  rating: 0,
  image: "",
};
const isFormOpen = ref(false);
const isEdited = ref(false);

const activeMovie = ref({ ...defaultMovieState });

const openForm = () => {
  activeMovie.value = { ...defaultMovieState };
  isFormOpen.value = true;
  isEdited.value = false;
};

const addMovieCard = (newMovie) => {
  movieList.value.push({
    id: movieList.value.length + 1,
    ...newMovie,
  });
  isFormOpen.value = false;
  isEdited.value = false;
};

const handleRemoveRating = () => {
  movieList.value.forEach((item) => {
    item.rating = null;
  });
};
const removeMovieCard = (selectedCardId) => {
  movieList.value = movieList.value.filter(
    (movie) => movie.id !== selectedCardId,
  );
};
const editMovieCard = (selectedCardId) => {
  const targetMovie = movieList.value.find((m) => m.id === selectedCardId);
  isFormOpen.value = true;
  isEdited.value = true;
  activeMovie.value = { ...targetMovie };
};

const handleSaveEdit = (updatedMovie) => {
  const index = movieList.value.findIndex((m) => m.id === updatedMovie.id);
  if (index !== -1) {
    movieList.value[index] = { ...updatedMovie };
  }
  isFormOpen.value = false;
  isEdited.value = false;
};

const totalMovies = computed(() => movieList.value.length);
const totalRating = computed(() =>
  movieList.value.reduce((acc, item) => item.rating + acc, 0),
);
</script>

<template>
  <div class="container">
    <div class="indicator">
      <p>Total movies:</p>
      <span>{{ totalMovies }}</span>
    </div>

    <div class="indicator">
      <p>Total rating:</p>
      <span>{{ totalRating }}</span>
    </div>

    <button class="add-btn" @click="handleRemoveRating">Remove raiting</button>

    <button class="add-btn" @click="openForm">Add Movie</button>
    <SelectedMovieForm
      v-if="isFormOpen"
      :id="activeMovie.id"
      :name="activeMovie.name"
      :description="activeMovie.description"
      :genres="activeMovie.genres"
      :rating="activeMovie.rating"
      :image="activeMovie.image"
      :isEdited="isEdited"
      @add-movie="addMovieCard"
      @edit-movie="handleSaveEdit"
      @cancel="isFormOpen = false"
    />

    <div class="movie-list">
      <MovieCard
        v-for="movie in movieList"
        :key="movie.id"
        :name="movie.name"
        :description="movie.description"
        :rating="movie.rating"
        :genres="movie.genres"
        :image="movie.image"
        :id="movie.id"
        @remove-movie="removeMovieCard(movie.id)"
        @edit-movie="editMovieCard(movie.id)"
      />
    </div>
  </div>
</template>

<style>
.container {
  display: flex;
  flex-direction: column;
  gap: 16px;
  padding: 16px;
  width: 100%;
  position: relative;
}

.movie-list {
  display: flex;
  flex-wrap: nowrap;
  gap: 16px;
}

.add-btn {
  padding: 8px 16px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  width: 20%;
}

.indicator {
  display: flex;
  gap: 4px;
  width: 40%;
  color: #fff;
}
</style>
