<template>
  <section class="albums-section">
    <h2>Album Saya</h2>
    <div class="select-user">
      <label for="user-select">Pilih Pengguna Album:</label>
      <select id="user-select" v-model="selectedUser" @change="fetchAlbums" class="select-box">
        <option v-for="user in users" :key="user.id" :value="user.id">{{ user.name }}</option>
      </select>
    </div>
    <div v-if="albums.length > 0" class="user-albums">
      <h3>Album Saya: {{ selectedUserName }}</h3>
      <ul>
        <li v-for="album in albums" :key="album.id">
          <router-link :to="{ name: 'albumDetail', params: { id: album.id } }">
            {{ album.title }}
          </router-link>
        </li>
      </ul>
    </div>
  </section>
</template>

<script setup>
import { ref, onMounted } from 'vue'

const selectedUser = ref(null)
const users = ref([])
const albums = ref([])
const selectedUserName = ref('')

const fetchUsers = async () => {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/users')
    users.value = await response.json()
  } catch (error) {
    console.error('Error fetching users:', error)
  }
}

const fetchAlbums = async () => {
  if (!selectedUser.value) return
  try {
    const response = await fetch(`https://jsonplaceholder.typicode.com/albums?userId=${selectedUser.value}`)
    albums.value = await response.json()
    selectedUserName.value = users.value.find(user => user.id === selectedUser.value)?.name || ''
  } catch (error) {
    console.error('Error fetching albums:', error)
  }
}

onMounted(fetchUsers)
</script>

<style scoped>
.albums-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  background-image: url('https://images.pexels.com/photos/1557489/pexels-photo-1557489.jpeg');
  background-size: cover;
  background-position: center;
  min-height: 100vh;
}

h2 {
  margin-bottom: 1.5rem;
  color: rgb(0, 14, 12);
}

.select-user {
  margin-bottom: 1.5rem;
}

.select-user label {
  color: rgb(0, 0, 0);
  margin-right: 1rem;
}

.select-box {
  padding: 8px 12px;
  font-size: 16px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #f8f8f8;
  transition: border-color 0.3s, background-color 0.3s;
}

.select-box:focus {
  outline: none;
  border-color: #007bff;
  background-color: white;
}

.user-albums {
  text-align: center;
  font-family: 'Times New Roman, sans-serif';
}

.user-albums ul {
  list-style-type: none;
  padding: 0;
}

.user-albums li {
  background-color: rgb(0, 0, 0);
  padding: 1rem;
  margin: 0.5rem 0; 
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(8, 0, 0, 0.1);
}

.user-albums li a {
  text-decoration: none;
  color: #007bff;
}
</style>
