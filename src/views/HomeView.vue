<template>
  <main class="container text-white">
    <div class="pt-4 mb-8 relative">
      <input
        v-model="searchQuery"
        @input="getSearchResults"
        type="text"
        placeholder="Search for a user by username..."
        class="text-lg py-2 px-1 w-full bg-transparent border-b focus:border-search-secondary focus:outline-none focus:shadow-[0px_1px_0_0_#004E71]"
      />
    </div>
    <div v-if="searchError" class="text-[#F0B4B4]">{{ searchError }}</div>
    <div v-else-if="searchQuery && searchResults.length === 0">
      No results found.
    </div>
    <div v-else>
      <div
        v-for="user in searchResults"
        :key="user.id"
        class="mb-4 flex justify-center items-center flex-col bg-search-secondary p-4 rounded-md shadow-md"
      >
        <h2 class="text-xl font-bold mb-2">{{ user.login }}</h2>
        <img
          :src="user.avatar_url"
          :alt="`Avatar of ${user.login}`"
          class="w-20 h-20 rounded-full shadow-md mx-auto md:mx-0 cursor-pointer"
          @click="toggleUserInfo(user)"
        />
        <div v-if="user.showInfo" class="mt-2">
          <p class="text-lg">Name: {{ user.name }}</p>
          <p class="text-lg">Bio: {{ user.bio }}</p>
          <p class="text-lg">
            URL:
            <a
              :href="user.html_url"
              target="_blank"
              rel="noopener noreferrer"
              >{{ user.html_url }}</a
            >
          </p>
        </div>
      </div>
    </div>
  </main>
</template>

<script setup>
import { ref } from "vue";
import axios from "axios";

const searchQuery = ref("");
const queryTimeout = ref(null);
const searchResults = ref([]);
const searchError = ref(null);

const getSearchResults = async () => {
  clearTimeout(queryTimeout.value);
  queryTimeout.value = setTimeout(async () => {
    if (searchQuery.value !== "") {
      try {
        const result = await axios.get(
          `https://api.github.com/users/${searchQuery.value}`
        );
        searchResults.value = [result.data].map((user) => ({
          ...user,
          showInfo: false,
        }));
        searchError.value = null;
      } catch (error) {
        console.error("Error fetching search results:", error);
        searchError.value = "User not found. Please try again.";
      }
      return;
    }
    searchResults.value = [];
  }, 300);
};

const toggleUserInfo = (user) => {
  user.showInfo = !user.showInfo;
};
</script>
