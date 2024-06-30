<template>
  <div class="flex flex-row gap-3 flex-1 justify-center flex-wrap">
    <div
      v-for="(
        { id, name, genres, description, inTheaters, image, rating }, index
      ) in movies"
      :key="id"
      class="max-w-xs h-100 bg-white border border-gray-200 rounded-lg shadow dark:bg-gray-800 dark:border-gray-700 flex flex-col"
    >
      <div class="w-full h-96 relative">
        <div class="absolute top-0 right-0">
          <p v-if="rating > 0" class="absolute z-10 right-4 bottom-2">
            {{ rating }}
          </p>
          <p v-else class="absolute right-4 bottom-2">-</p>
          <StarIcon v-if="rating" class="size-10 text-yellow-500 relative" />
        </div>
        <img
          class="w-full h-full object-cover rounded-t-lg"
          :src="image"
          :alt="name"
        />
      </div>
      <div class="p-5 bg-white flex-1 flex flex-col">
        <h5
          class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-black"
        >
          {{ name }}
        </h5>
        <div class="flex flex-row gap-2 mb-3">
          <div
            class="font-normal text-white rounded-lg bg-indigo-500 px-2 h-7"
            v-for="genre in genres"
            :key="genre"
          >
            <span>{{ genre }} &nbsp;</span>
          </div>
        </div>
        <p class="text-black mb-4 flex-1">{{ description }}</p>
        <div class="flex flex-row items-center justify-between mt-auto">
          <div class="flex items-center">
            <p class="text-black">Rating: ({{ rating > 0 ? rating : "" }}/5)</p>
            <div class="ml-2 mt-1">
              <button
                :disabled="rating === star"
                @click="handleUserRating(index, star)"
                v-for="star in 5"
                :key="star"
              >
                <StarIcon
                  :class="[
                    'w-4 h-4',
                    rating < star ? 'text-gray-500' : 'text-yellow-500',
                  ]"
                />
              </button>
            </div>
          </div>
          <div class="flex gap-3">
            <PencilIcon
              @click="
                $emit('edit-movie', {
                  id,
                  name,
                  description,
                  image,
                  genres,
                  inTheaters,
                  rating,
                })
              "
              class="size-5 cursor-pointer"
            />
            <TrashIcon
              @click="handleRemoveMovie(index)"
              class="size-5 cursor-pointer"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
  import { StarIcon, PencilIcon, TrashIcon } from "@heroicons/vue/24/solid";

  const props = defineProps(["movies"]);
  const emits = defineEmits(["edit-movie"]);

  const handleUserRating = (index, value) => {
    props.movies[index].rating = value;
  };
  const handleRemoveMovie = (index) => {
    props.movies.splice(index, 1);
  };
</script>
