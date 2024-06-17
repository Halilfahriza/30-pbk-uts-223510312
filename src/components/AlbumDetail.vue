<template>
  <section class="album-detail-section">
    <h2>Detail Album</h2>
    <div v-if="album" class="album-detail">
      <h3>{{ album.title }}</h3>
      <div class="photo-cards">
        <div v-for="photo in photos" :key="photo.id" class="photo-card">
          <img :src="photo.thumbnailUrl" :alt="photo.title" />
          <p>{{ photo.title }}</p>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue'
import { useRoute } from 'vue-router'

const album = ref(null)
const photos = ref([])

const route = useRoute()
const albumId = route.params.id

const fetchAlbumDetail = async () => {
  try {
    const response = await fetch(`https://jsonplaceholder.typicode.com/albums/${albumId}`)
    album.value = await response.json()
    const photosResponse = await fetch(`https://jsonplaceholder.typicode.com/photos?albumId=${albumId}`)
    photos.value = await photosResponse.json()
  } catch (error) {
    console.error('Error fetching album details:', error)
  }
}

onMounted(fetchAlbumDetail)
</script>

<style scoped>
.album-detail-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  background-image: url('https://images.pexels.com/photos/264940/pexels-photo-264940.jpeg');
  background-size: cover;
  background-position: center;
  min-height: 100vh;
}

h2 {
  margin-bottom: 1.5rem;
  color: white;
}

.album-detail {
  text-align: center;
  font-family: 'Times New Roman, sans-serif';
}

.photo-cards {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 1rem;
}

.photo-card {
  width: 150px;
  background-color: white;
  border-radius: 8px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  padding: 1rem;
  text-align: left;
}

.photo-card img {
  width: 100%;
  border-radius: 4px;
  margin-bottom: 0.5rem;
}

.photo-card p {
  color: #333;
}
</style>
