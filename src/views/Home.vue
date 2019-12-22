<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <h2>Create New Product</h2>
    <p>Name: <input type="text" v-model="newProductName"></p> 
    <p>Description: <input type="text" v-model="newProductDescription"></p>
    <p>Price: <input type="text" v-model="newProductPrice"></p>
    <p>Image Url: <input type="text" v-model="newProductImageUrl"></p>
    <button v-on:click="addProduct()">Add New Product</button>
    <br>
    <p>Search for Products:<input v-model="searchTerm" list="names"></p>
    <datalist id="names">
      <option v-for="product in products">{{ product.name }}</option>
    </datalist>
    <button v-on:click="setSortAttribute('name')">Order by name</button>
    <button v-on:click="setSortAttribute('price')">Order by price</button>
    <!-- <div v-for="product in filterBy(products, searchTerm, 'name')" v-bind:class="{selected: product.selected}"> -->
    <div v-for="product in orderBy(products, sortAttribute)" v-bind:class="{selected: product.selected}">
      <button v-on:click="selectProduct(product)">Select Product</button>
      <p>{{ product.name }}</p>
      <p>Price: ${{ product.price }}</p>
      <img v-bind:src="product.image_url" width="300px" v-bind:alt="product.name">
      <br>
      <button v-on:click="toggleInfo(product)">See more info</button>
      <div v-if="currentProduct === product">
        <p>{{ product.description }}</p>
        <p>Update Product</p>
        <p>Name: <input type="text" v-model="product.name"></p>
        <p>Description: <input type="text" v-model="product.description"></p>
        <p>Price: <input type="text" v-model="product.price"></p>
        <p>Image Url: <input type="text" v-model="product.image_url"></p>
        <button v-on:click="updateProduct(product)">Update Product</button>
        <button v-on:click="destroyProduct(product)">Destroy Product</button>
      </div>
      <hr>
    </div>
  </div>
</template>

<style>
.selected {
  color: white;
  background-color: steelBlue;
  transition: background-color 1s ease;
}
</style>

<script>
import axios from "axios";
import Vue2Filters from "vue2-filters";

export default {
  mixins: [Vue2Filters.mixin],
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      products: [],
      newProductName: "",
      newProductDescription: "",
      newProductPrice: "",
      newProductImageUrl: "",
      currentProduct: {},
      searchTerm: "",
      sortAttribute: "name"
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
      if (this.currentProduct === theProduct) {
        this.currentProduct = null;
      } else { 
        this.currentProduct = theProduct;
      }
    },
    updateProduct: function(theProduct) {
      console.log(theProduct);

      var params = {
        name: theProduct.name,
        description: theProduct.description,
        price: theProduct.price,
        image_url: theProduct.image_url
      };
      axios.patch(`/api/products/${theProduct.id}`, params).then(response => {
        console.log(response.data);
        theProduct.name = response.data.name;
        theProduct.description = response.data.description;
        theProduct.price = response.data.price;
        theProduct.image_url = response.data.image_url;
      });
    },
    destroyProduct: function(theProduct) {
      console.log(theProduct);
      axios.delete(`api/products/${theProduct.id}`).then(reponse => {
        var index = this.products.indexOf(theProduct);
        console.log(index);
        this.products.splice(index, 1);
      });
    },
    selectProduct: function(theProduct) {
      // console.log('selecting product');
      theProduct.selected = !theProduct.selected;
      // console.log(theProduct);
    },
    setSortAttribute: function(theAttribute) {
      this.sortAttribute = theAttribute;
    }
  }
};
</script>