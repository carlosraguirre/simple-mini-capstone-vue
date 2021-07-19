/* eslint-disable prettier/prettier */
<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>Name: <input type="text" v-model="newProductParams.name"></p>
    <p>Price: <input type="text" v-model="newProductParams.price"></p>
    <p>Description: <input type="text" v-model="newProductParams.description"></p>
    <p>Image Url: <input type="text" v-model="newProductParams.image_url"></p>
    <button v-on:click="productsCreate()">Create Product</button>
    <div v-for="product in products" v-bind:key="product.id">
      <p>Name: {{ product.name }}</p>
      <p>Price: {{ product.price }}</p>
      <p>Image URL: {{ product.image_url }}</p>
      <img v-bind:src="product.image_url">
      <p><button v-on:click="productShow(product)">Show more info</button></p>
      <hr>
    </div>
    <dialog id="product-details">
      <form method="dialog">
        <p>Product:</p>
        <p>Name: {{ currentProduct.name }}</p>
        <p>Price: {{ currentProduct.price }}</p>
        <p>Description: {{ currentProduct.name }}</p>
  }     <p>Image URL: {{ currentProduct.image_url }}</p>
        <button>Close</button>
      </form>
    </dialog>
  </div>
</template>
<style></style>
<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Welcome to the simple app!",
      products: [],
      newProductParams: {},
      errors: [],
      currentProduct: {}
    };
  },
  created: function () {
    this.productsIndex();
  },
  methods: {
    productsIndex: function () {
      console.log("in products index");
      axios.get("http://localhost:3000/products").then((response) => {
        console.log(response.data);
        this.products = response.data;
      });
    },
    productsCreate: function () {
      console.log("create product")
      var productParams = {
        name: this.newProductParams.name,
        price: this.newProductParams.price,
        description: this.newProductParams.description,
        image_url: this.newProductParams.image_url,
      };
      axios.post("http://localhost:3000/products", productParams).then((response) => {
        console.log(response.data);
        this.products.push(response.data);
        this.newProductParams = {}
      });
    },
    productShow: function (theProduct) {
      console.log(theProduct);
      this.currentProduct = theProduct;
      console.log("showing product");
      document.querySelector("#product-details").showModal();
    }
  }
};
</script>