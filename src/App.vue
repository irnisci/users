<template>
  <div>
    <input v-model="username" @keyup.enter="getUserData" class="border p-2 mb-2 outline-none m-5 " placeholder="GitHub Benutzername" />
    <button @click="getUserData" class="bg-blue-500 text-white px-4 py-2 rounded ">Daten abrufen</button>

    <div v-if="user">
      <div class="bg-white p-6 rounded-lg shadow-md">
        <div class="flex items-center mb-4">
          <img :src="user.avatar_url" :alt="user.name" class="w-12 h-12 rounded-full mr-4">
          <div>
            <h2 class="text-lg font-bold">{{ user.name }}</h2>
            <p class="text-gray-600">Anzahl der öffentlichen Repositories: {{ user.public_repos }}</p>
          </div>
        </div>
        <h3 class="text-lg font-bold mb-2">Repositories:</h3>
        <div class="grid grid-cols-1 gap-4 max-w-screen-sm">
          <div v-for="repo in repositories" :key="repo.id" class="bg-gray-100 p-4 shadow-md rounded-lg">
            <h4 class="text-lg font-semibold">{{ repo.name }}</h4>
            <p class="mt-2 text-gray-600">{{ repo.description }}</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import axios from 'axios';

const username = ref('');
const user = ref(null);
const repositories = ref([]);

const getUserData = async () => {
  try {
    const response = await axios.get(`https://api.github.com/users/${username.value}`, {
      headers: {
        Authorization: "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", 
      },
    });
    user.value = response.data;

    const reposResponse = await axios.get(response.data.repos_url, {
      headers: {
        Authorization: "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", 
      },
    });
    repositories.value = reposResponse.data;
  } catch (error) {
    console.error('Fehler beim Abrufen der Daten:', error);
  }
};
</script>