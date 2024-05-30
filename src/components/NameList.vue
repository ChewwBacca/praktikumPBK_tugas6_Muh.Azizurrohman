<template>
  <div class="container">
    <div class="konten">
      <h1>✦ CRUD ✦</h1>
      <input v-model="newName" placeholder="Enter name" />
      <button @click="addName">Add Name</button>
      <ol>
        <li v-for="name in names" :key="name.id">
          {{ name.name }}
          <button @click="deleteName(name.id)">Delete</button>
        </li>
      </ol>
    </div>
  </div>
  </template>
  
  <script>
  import axios from 'axios';
  
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
          const response = await axios.get('http://localhost:3000/names');
          this.names = response.data;
        } catch (error) {
          console.error('Error fetching names:', error);
        }
      },
      async addName() {
        if (this.newName.trim() === '') return;
        try {
          const response = await axios.post('http://localhost:3000/names', { name: this.newName });
          this.names.push(response.data);
          this.newName = '';
        } catch (error) {
          console.error('Error adding name:', error);
        }
      },
      async deleteName(id) {
        try {
          await axios.delete(`http://localhost:3000/names/${id}`);
          this.names = this.names.filter(name => name.id !== id);
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
    margin-left: 700px;
    margin-right: 700px;
    margin-top: 400px;
    
  }
  .konten{
    padding: 30px;
    color: solid black;
    font-family: 'Courier New', Courier, monospace;
  }
  </style>
  