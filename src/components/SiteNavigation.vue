<template>
  <header class="sticky top-0 bg-search-primary shadow-lg">
    <nav
      class="container flex flex-col sm:flex-row items-center gap-4 text-white py-6"
    >
      <RouterLink :to="{ name: 'home' }">
        <div class="flex items-center gap-3">
          <i class="fa-solid fa-users text-2xl"></i>
          <p class="text-2xl">Github User Search</p>
        </div>
      </RouterLink>

      <div class="flex flex-1 gap-3 justify-end">
        <i
          class="fa-solid fa-circle-info text-xl hover:text-search-secondary duration-150 cursor-pointer"
          @click="toggleModal"
        ></i>
        <i
          class="fa-solid fa-rotate-right text-xl hover:text-search-secondary duration-150 cursor-pointer"
          @click="reloadPage"
        ></i>
      </div>

      <BaseModal :modalActive="modalActive" @close-modal="toggleModal">
        <div class="text-black">
          <p>
            To use this application, simply type a GitHub username into the
            search bar. The user's profile information, including their name,
            bio, url, and avatar, will be displayed. You can also click on the
            user's url to visit their profile page.
          </p>
        </div>
      </BaseModal>
    </nav>
  </header>
</template>

<script setup>
import { RouterLink } from "vue-router";
import { ref } from "vue";
import BaseModal from "./BaseModal.vue";

const modalActive = ref(null);
const toggleModal = () => {
  modalActive.value = !modalActive.value;
};

const reloadPage = () => {
  location.reload();
};
</script>
