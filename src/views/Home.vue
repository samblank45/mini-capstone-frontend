<template>
  <div class="home">
    <h1> New Product </h1>
    <ul>
      <li v-for="error in createErrors"> {{ error }} </li>
    </ul>

    <div>
      Name: <input type="text" v-model="newProductName"> <br>
      description: <input type="text" v-model="newProductDescription"> <br>
      price: <input type="text" v-model="newProductPrice"> <br>
      image_Url: <input type="text" v-model="newProductImageUrl"> <br>
      <button v-on:click="createProduct()">Create</button>
    </div>

    <div v-for="product in products">
      <h1> {{ product.name }} </h1>
      <img v-bind:src="product.images[0].url" v-bind:alt="product.name">
      <h3> {{product.description }} </h3>
      <h3> price: {{ product.price }} </h3>
      <button v-on:click="showProduct(product)">More Info</button>
    </div>

    <dialog id="product-details"> 
      <form method="dialog">
        <h1> product Info </h1>
        <ul>
          <li v-for="error in updateErrors"> {{ error }} </li>
        </ul>
        <p>Image Url: <input type="text" v-model="currentProduct.image_url"></p>
        <p>Name: <input type="text" v-model="currentProduct.name"></p>
        <p>Description: <input type="text" v-model="currentProduct.description"></p>
        <p>Price: <input type="text" v-model="currentProduct.price"></p>
        <button v-on:click="updateProduct(currentProduct)">Update</button>
        <button v-on:click="destroyProduct(currentProduct)">Destroy</button>
        <button>close</button>
      </form>
    </dialog>

  </div>
</template>
<style>
img {
  width: 250px;
}
button {
  color: rgb(23, 23, 143);
}

#product-details {
  color: red;
}
</style>
<script>
import axios from "axios";
export default {
  data: function() {
    return {
      newProductName: "",
      newProductPrice: "",
      newProductDescription: "",
      newProductImageUrl: "",
      products: [],
      createErrors: [],
      updateErrors: [],
      currentProduct: {}
    };
  },
  created: function() {
    this.indexProducts();
  },
  methods: {
    indexProducts: function() {
      axios.get("/api/products").then(response => {
        console.log("all products:", response.data);
        this.products = response.data;
      });
    },
    createProduct: function() {
      var params = {
        name: this.newProductName,
        price: this.newProductPrice,
        description: this.newProductDescription,
        image_url: this.newProductImageUrl
      };

      axios
        .post("/api/products", params)
        .then(response => {
          console.log("success", response.data);
          this.products.push(response.data);
        })
        .catch(error => {
          console.log(error.response.data.errors);
          this.errors = error.response.data.errors;
        });
    },
    showProduct: function(product) {
      console.log(product);
      this.currentProduct = product;
      document.querySelector("#product-details").showModal();
    },
    updateProduct: function(product) {
      var params = {
        name: product.name,
        price: product.price,
        description: product.description,
        image_url: product.image_url
      };
      axios
        .patch(`/api/products/${product.id}`, params)
        .then(response => {
          console.log("successfully updated", response.data);
        })
        .catch(error => {
          console.log(error.response.data.errors);
          this.updateErrors = error.response.data.errors;
        });
    },
    destroyProduct: function(product) {
      axios.delete(`/api/products/${product.id}`).then(response => {
        console.log("successfully destroyed", response.data);
        var index = this.products.indexOf(product);
        this.products.splice(index, 1);
      });
    }
  }
};
</script>