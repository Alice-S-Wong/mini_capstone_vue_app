<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2>Create New Product</h2>
    <p>Name:<input type="text" v-model="newProductName"></p> 
    <p>Description:<input type="text" v-model="newProductDescription"></p>
    <p>Price:<input type="text" v-model="newProductPrice"></p>
    <p>Image Url:<input type="text" v-model="newProductImageUrl"></p>
    <button v-on:click="addProduct()">Add New Product</button>
    <div v-for="product in products">
      <p>{{ product.name }}</p>
      <p>Price: ${{ product.price }}</p>
      <img v-bind:src="product.image_url" width="300px" v-bind:alt="product.name">
      <br>
      <button v-on:click="toggleInfo(product)">See more info</button>
      <div v-if="currentProduct === product">
        <p>{{ product.description }}</p>
      </div>
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
      products: [],
      newProductName: "",
      newProductDescription: "",
      newProductPrice: "",
      newProductImageUrl: "",
      currentProduct: {}
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
        name: this.newProductName,
        description: this.newProductDescription,
        price: this.newProductPrice,
        image_url: this.newProductImageUrl
      };
      console.log(params);
      axios.post("/api/products", params).then(response => {
        console.log(response.data);
        this.products.push(response.data);
        this.newProductName = "";
        this.newProductDescription = "";
        this.newProductPrice = "";
        this.newProductImageUrl = "";
      }).catch(error => console.log(error.response));
    },
    toggleInfo: function(theProduct) {
      console.log(theProduct);
      this.currentProduct = theProduct;
    }
  }
};
</script>