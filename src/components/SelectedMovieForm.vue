<script setup>
import { reactive, watch } from "vue";

const emit = defineEmits(["add-movie", "cancel", "edit-movie"]);

const props = defineProps({
  id: Number,
  name: String,
  description: String,
  genres: Array,
  rating: Number,
  image: String,
  isEdited: Boolean,
});
const form = reactive({
  id: props.id || null,
  name: props.name || "",
  description: props.description || "",
  genres: props.genres || [],
  rating: props.rating || 0,
  image: props.image || "",
});

watch(
  () => props,
  (newProps) => {
    form.id = newProps.id || null;
    form.name = newProps.name || "";
    form.description = newProps.description || "";
    form.genres = newProps.genres ? [...newProps.genres] : [];
    form.rating = newProps.rating || 0;
    form.image = newProps.image || "";
  },
  { immediate: true, deep: true },
);

const submitMovie = () => {
  if (props.isEdited) {
    emit("edit-movie", { ...form });
  } else {
    emit("add-movie", {
      ...form,
    });
  }
};
</script>

<template>
  <form class="container" @submit.prevent="submitMovie">
    <h3>Select a movie to view its details.</h3>

    <p>Name</p>
    <input type="text" v-model="form.name" placeholder="Name" class="select" />

    <p>Description</p>
    <textarea
      v-model="form.description"
      placeholder="Description"
      class="select"
    ></textarea>

    <p>Genres</p>
    <select v-model="form.genres" multiple class="select">
      <option value="Action">Action</option>
      <option value="Comedy">Comedy</option>
      <option value="Drama">Drama</option>
      <option value="Horror">Horror</option>
      <option value="Sci-Fi">Sci-Fi</option>
    </select>

    <p>Rating</p>
    <input
      type="number"
      v-model="form.rating"
      placeholder="Rating"
      class="select"
    />

    <p>Image</p>
    <input
      type="text"
      v-model="form.image"
      placeholder="Image URL"
      class="select"
    />

    <button type="submit" class="btn">
      {{ isEdited ? "Update Movie" : "Create Movie" }}
    </button>
    <button type="button" class="btn cancel" @click="emit('cancel')">
      Cancel
    </button>
  </form>
</template>

<style scoped>
.container {
  display: flex;
  flex-direction: column;
  gap: 16px;
  padding: 24px;
  width: 80%;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: rgb(17 24 39 / var(--tw-bg-opacity, 1));
  z-index: 100;
  border-radius: 20px;
  color: white;
}

.btn {
  background-color: #4caf50;
  color: white;
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}
.btn.cancel {
  background-color: #f44336;
}

.select {
  color: red;
}
</style>
