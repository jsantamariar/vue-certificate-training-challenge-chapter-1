<template>
  <form class="bg-blue-500 p-5 rounded-lg shadow-lg relative w-96">
    <div class="mb-5">
      <label
        for="name"
        class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
      >
        Name
      </label>
      <input
        type="text"
        id="name"
        v-model="formData.name"
        class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
        required
      />
    </div>
    <div class="mb-5">
      <label
        for="description"
        class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
      >
        Description
      </label>
      <input
        type="text"
        id="description"
        v-model="formData.description"
        class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
        required
      />
    </div>
    <div class="mb-5">
      <label
        for="image"
        class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
      >
        Image
      </label>
      <input
        type="text"
        id="image"
        v-model="formData.image"
        class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
        required
      />
    </div>
    <div class="mb-5">
      <label
        for="genres"
        class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
      >
        Genres
      </label>
      <select
        id="genres"
        v-model="formData.genres"
        required
        multiple
        class="w-52"
      >
        <option value="Drama">Drama</option>
        <option value="Crime">Crime</option>
        <option value="Action">Action</option>
        <option value="Comedy">Comedy</option>
      </select>
    </div>
    <div class="flex items-start mb-5">
      <div class="flex items-center h-5">
        <input
          id="inTheaters"
          type="checkbox"
          v-model="formData.inTheaters"
          class="w-4 h-4 border border-gray-300 rounded bg-gray-50 focus:ring-3 focus:ring-blue-300 dark:bg-gray-700 dark:border-gray-600 dark:focus:ring-blue-600 dark:ring-offset-gray-800 dark:focus:ring-offset-gray-800"
        />
      </div>
      <label
        for="inTheaters"
        class="ml-2 text-sm font-medium text-gray-900 dark:text-gray-300"
      >
        In theaters
      </label>
    </div>
    <div class="flex justify-between w-100">
      <button
        type="button"
        class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm w-full sm:w-auto px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
        @click="handleCancel"
      >
        Cancel
      </button>
      <button
        @click.prevent="handleSubmit"
        type="submit"
        class="text-white bg-blue-700 hover:bg-blue-800 focus:ring-4 focus:outline-none focus:ring-blue-300 font-medium rounded-lg text-sm w-full sm:w-auto px-5 py-2.5 text-center dark:bg-blue-600 dark:hover:bg-blue-700 dark:focus:ring-blue-800"
      >
        {{ movieToEdit ? "Edit" : "Add movie" }}
      </button>
    </div>
  </form>
</template>

<script setup>
  import { reactive, watch } from "vue";

  const props = defineProps(["movieToEdit"]);
  const emit = defineEmits(["close", "add-movie", "edit-movie"]);

  const formData = reactive({
    id: "",
    name: "",
    description: "",
    image: "",
    genres: [],
    inTheaters: false,
    rating: 0,
  });

  const resetForm = () => {
    formData.id = "";
    formData.name = "";
    formData.description = "";
    formData.image = "";
    formData.genres = [];
    formData.inTheaters = false;
    formData.rating = 0;
  };

  watch(
    () => props.movieToEdit,
    (newVal) => {
      if (newVal) {
        Object.assign(formData, newVal);
      } else {
        resetForm();
      }
    },
    { immediate: true }
  );

  const handleSubmit = () => {
    const formDataCopy = { ...formData };
    if (props.movieToEdit) {
      emit("edit-movie", formDataCopy);
    } else {
      formDataCopy.id = Math.random().toString(36).substr(2, 9);
      emit("add-movie", formDataCopy);
    }
    emit("close");
    resetForm();
  };

  const handleCancel = () => {
    emit("close");
    resetForm();
  };
</script>
