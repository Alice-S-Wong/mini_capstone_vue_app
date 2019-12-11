<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <button v-on:click="addProduct()">Add New Product</button>
    <div v-for="product in products">
      <p>{{ product.name }}</p>
      <p>Price: ${{ product.price }}</p>
      <p>{{ product.description }}</p>
      <img v-bind:src="product.image_url" width="300px" v-bind:alt="product.name">
      <hr>
    </div>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      products: []
    };
  },
  created: function() {
    axios.get("/api/products").then(response => {
      console.log(response.data);
      this.products = response.data;
    });
  },
  methods: {
    addProduct: function() {
      console.log('adding new product');
      var params = {
        name: "Rocket Artillery",
        description: "Artillery that shoots rockets. It's actually rocket science this time! Unlike flamethrowers, this one isn't meant for kids.",
        price: 5100000,
        image_url: "https://upload.wikimedia.org/wikipedia/commons/8/83/HIMARS_-_missile_launched.jpg"
      };
      axios.post("/api/products", params).then(response => {
        console.log(response.data);
      });
    }
  }
};
</script>