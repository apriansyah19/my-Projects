<template>
  <div>
    <Navbar />
    <div class="container">
      <div class="row mt-4">
        <div class="col">
          <h2>Daftar <strong>Makanan</strong></h2>
        </div>
      </div>

      <div class="row mt-3">
        <div class="col">
          <div class="input-group mb-3">
            <input
              v-model="searchFoods"
              type="text"
              class="form-control"
              placeholder="Search Your Favorite Food"
              aria-label="Search"
              aria-describedby="basic-addon2"
              @keyup="getSearchFood"
            />
            <div class="input-group-append">
              <span class="input-group-text" id="basic-addon2"
                ><b-icon-search> </b-icon-search></span
              >
            </div>
          </div>
        </div>
      </div>

      <div class="row mb-4">
        <div
          class="col-md-4 mt-4"
          v-for="product in products"
          :key="product.id"
        >
          <Product :cardProduct="product" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import Navbar from "@/components/Navbar.vue";
import Product from "@/components/Product.vue";
import axios from "axios";

export default {
  name: "Foods",
  components: {
    Navbar,
    Product,
  },
  data() {
    return {
      products: [],
      searchFoods: '',
    };
  },
  methods: {
    setProducts(data) {
      this.products = data;
    },
    getSearchFood() {
        axios
      .get("http://localhost:3000/products?q="+this.searchFoods)
      .then((response) => {
        // handle success
        this.setProducts(response.data);
      })
      .catch((error) => {
        // handle error
        console.log(error, "error");
      });
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/products")
      .then((response) => {
        // handle success
        this.setProducts(response.data);
      })
      .catch((error) => {
        // handle error
        console.log(error, "error");
      });
  },
};
</script>
<style>
</style>