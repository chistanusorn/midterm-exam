<template>
  <q-page class="flex flex-center column">
    <h3>Backend Data</h3>
    <div v-if="loading">Loading...</div>
    <div v-else>
       <p><strong>Git:</strong> {{ apiData.git.title }} - {{ apiData.git.detail }}</p>
       <p><strong>Docker:</strong> {{ apiData.docker.title }} - {{ apiData.docker.detail }}</p>
    </div>
    <q-btn color="primary" @click="fetchData" label="Refresh" />
  </q-page>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

const apiData = ref({ git: {}, docker: {} });
const loading = ref(true);

const fetchData = async () => {
  loading.value = true;
  try {
    // ใช้ URL ตรงๆ หรือผ่าน env ก็ได้ แต่เพื่อความง่ายในการเทสเบื้องต้น
    const url = import.meta.env.VITE_API_URL || 'http://localhost:3000';
    const response = await axios.get(url + '/api/demo');
    apiData.value = response.data;
  } catch (error) {
    console.error('API Error:', error);
  } finally {
    loading.value = false;
  }
};

onMounted(fetchData);
</script>