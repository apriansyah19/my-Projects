<template>
  <div class="home">
    <Navbar />
    <div class="container">
      <Hero />
      <div class="row mt-4">
        <div class="col">
          <h2>Best <strong>Foods</strong></h2>
        </div>
        <div class="col">
          <router-link to="/foods" class="btn btn-success float-right"
            ><b-icon-eye> </b-icon-eye> Lihat Semua</router-link
          >
        </div>
      </div>
      <div class="row mb-4">
        <div class="col-md-3 mt-4" v-for="product in products" :key="product.id">
          <Product :cardProduct="product"/>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Navbar from "@/components/Navbar.vue";
import Hero from "@/components/Hero.vue";
import Product from "@/components/Product.vue";
import axios from "axios";

export default {
  name: "Home",
  components: {
    Navbar,
    Hero,
    Product,
  },
  data() {
    return {
      products: [],
    };
  },
  methods: { 
    setProducts(data) {
      this.products = data;
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/best-products")
      .then(
        response => {
        // handle success 
        this.setProducts(response.data)
      })
      .catch(error => {
        // handle error
        console.log(error, 'error');
      });
  },
};
</script>
