<template>
    <div>
        <h1>CRUD with json-server</h1>
        <form @submit.prevent="addItem">
          <input v-model="newItem" placeholder="Add new item" />
          <button type="submit">Add</button>
        </form>
        <ul>
          <li v-for="item in items" :key="item.id">
            {{ item.name }}
            <button @click="deleteItem(item.id)">Delete</button>
          </li>
        </ul>
      </div>
  </template>
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        items: [],
        newItem: ''
      };
    },
    created() {
      this.fetchItems();
    },
    methods: {
      async fetchItems() {
        try {
          const response = await axios.get('http://localhost:3000/items');
          this.items = response.data;
        } catch (error) {
          console.error(error);
        }
      },
      async addItem() {
        if (this.newItem.trim() === '') return;
        try {
          const response = await axios.post('http://localhost:3000/items', { name: this.newItem });
          this.items.push(response.data);
          this.newItem = '';
        } catch (error) {
          console.error(error);
        }
      },
      async deleteItem(id) {
        try {
          await axios.delete(`http://localhost:3000/items/${id}`);
          this.items = this.items.filter(item => item.id !== id);
        } catch (error) {
          console.error(error);
        }
      }
    }
  };
  </script>
  <style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }
  
  nav {
    padding: 30px;
  }
  
  nav a {
    font-weight: bold;
    color: #2c3e50;
  }
  
  nav a.router-link-exact-active {
    color: #42b983;
  }
  </style>
  