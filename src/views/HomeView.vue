<script setup>
import { ref } from "vue";
import axios from "axios";
const searchQuery = ref("");
const queryTimeout = ref(null);
const searchResults = ref(null);

const getSearchResults = () => {
  clearTimeout(queryTimeout.value);
  console.log(searchQuery.value);
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value !== "") {
      const result = axios.getSearchResults(
        `https://api.github.com/users/${searchQuery.value}`
      );
      searchResults.value = result;
      return;
    }
    searchResults.value = null;
  }, 300);
};
</script>

<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input
        v-model="searchQuery"
        type="text"
        placeholder="Search for a user"
        class="py-2 px-1 w-full bg-transparent border-b focus:border-search-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]"
      />
    </div>
  </main>
</template>
