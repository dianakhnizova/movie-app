<script setup>
import { computed, ref, watch } from "vue";
import { StarIcon } from "@heroicons/vue/24/solid";

const emit = defineEmits(["remove-movie", "edit-movie"]);

const props = defineProps({
  name: String,
  genres: Array,
  description: String,
  rating: Number,
  image: String,
  id: String,
});

const countCurrentStars = ref(5);
const currentRating = ref(props.rating);
const genres = computed(() => props.genres.map((genre) => genre).join(", "));

const handleClick = (selectedStar) => {
  currentRating.value = selectedStar;
};

const handleRemoveMovie = () => {
  emit("remove-movie", props.id);
};

const handleEditMovie = () => {
  emit("edit-movie", props.id);
};

watch(
  () => props.rating,
  (newVal) => {
    currentRating.value = newVal;
  },
);
</script>

<template>
  <div>
    <div class="card-container">
      <div class="img-container">
        <img class="movie-img" :src="props.image" :alt="props.name" />
        <div class="count-star-container">
          <StarIcon :class="{ active: currentRating > 0 }" />
          <span class="rating">{{ currentRating }}</span>
        </div>
      </div>

      <div class="info-container">
        <h3 class="title">{{ name }}</h3>
        <span class="style-movie">{{ genres }}</span>
        <p class="description">{{ description }}</p>
        <div class="btn-container">
          Raiting
          <button
            v-for="star in countCurrentStars"
            :key="`star-${star}`"
            @click="handleClick(star)"
            :disabled="star === currentRating"
          >
            <StarIcon class="star" :class="{ active: star <= currentRating }" />
          </button>
        </div>
      </div>

      <div class="btn-container-edit">
        <button class="btn" @click="handleEditMovie">Edit</button>
        <button class="btn" @click="handleRemoveMovie">Remove</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.card-container {
  display: flex;
  flex-direction: column;
  padding: 8px;
  border-radius: 8px;
  border: 1px solid #ccc;
  color: #fff;
}

.img-container {
  width: 100%;
  height: 200px;
  overflow: hidden;
  position: relative;
}

.movie-img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.info-container {
  display: flex;
  flex-direction: column;
  padding: 8px;
  gap: 8px;
}

.style-movie {
  border: 1px solid #ccc;
  padding: 4px 8px;
  border-radius: 4px;
}

.star {
  width: 20px;
  height: 20px;
  color: #6b7280;
  transition: color 0.2s;
}

.active {
  color: #eab308;
}

.count-star-container {
  display: flex;
  align-items: center;
  gap: 4px;
  position: absolute;
  top: 4px;
  right: 4px;
  width: 40px;
  height: 40px;
}

.rating {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  color: black;
}

.btn-container-edit {
  display: flex;
  gap: 8px;
  align-items: center;
}

.btn {
  display: flex;
  align-items: center;
  padding: 8px 16px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  width: 50%;
}
</style>
