<template>
  <div class="container">
    <div class="konten">
      <h1>✦ CRUD ✦</h1>
    <input v-model="newName" placeholder="Enter name" />
    <button @click="addName">Add Name</button>
    <ul>
      <li v-for="name in names" :key="name.id">
        {{ name.name }}
        <button @click="deleteName(name.id)">Delete</button>
      </li>
    </ul>
  </div>
</div>
</template>

<script>
import axios from 'axios';

const JSONBIN_URL = 'https://api.jsonbin.io/v3/b/6658ab95ad19ca34f871b940';
const JSONBIN_SECRET_KEY = '$2a$10$DAyUrkEOEvYb0od5x9pA8OyfLfBiXtTcL03DHnhjlHY06HDACnJp.';

export default {
  data() {
    return {
      newName: '',
      names: []
    };
  },
  methods: {
    async fetchNames() {
      try {
        const response = await axios.get(`${JSONBIN_URL}/latest`, {
          headers: {
            'X-Master-Key': JSONBIN_SECRET_KEY
          }
        });
        this.names = response.data.record.names;
      } catch (error) {
        console.error('Error fetching names:', error);
      }
    },
    async addName() {
      if (this.newName.trim() === '') return;
      try {
        this.names.push({ id: Date.now(), name: this.newName });
        await axios.put(JSONBIN_URL, { names: this.names }, {
          headers: {
            'Content-Type': 'application/json',
            'X-Master-Key': JSONBIN_SECRET_KEY
          }
        });
        this.newName = '';
      } catch (error) {
        console.error('Error adding name:', error);
      }
    },
    async deleteName(id) {
      try {
        this.names = this.names.filter(name => name.id !== id);
        await axios.put(JSONBIN_URL, { names: this.names }, {
          headers: {
            'Content-Type': 'application/json',
            'X-Master-Key': JSONBIN_SECRET_KEY
          }
        });
      } catch (error) {
        console.error('Error deleting name:', error);
      }
    }
  },
  created() {
    this.fetchNames();
  }
};
</script>

<style>
input {
  margin-right: 10px;
  border-radius: 5px;
}
button {
  margin-left: 15px;
  color: black;
  border-radius: 10px;
  margin-bottom: 5px;
}
.container{
  border: 4px solid black;
  border-radius: 25px;
  background-color: #F5F5F5;

  
}
.konten{
  padding: 30px;
  color: solid black;
  font-family: 'Courier New', Courier, monospace;
}
</style>
