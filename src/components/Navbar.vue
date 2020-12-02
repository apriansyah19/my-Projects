<template>
  <nav class="navbar navbar-expand-lg navbar-light bg-light">
    <div class="container">
      <a class="navbar-brand" href="#">Satria Resto</a>
      <button
        class="navbar-toggler"
        type="button"
        data-toggle="collapse"
        data-target="#navbarSupportedContent"
        aria-controls="navbarSupportedContent"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>

      <div class="collapse navbar-collapse" id="navbarSupportedContent">
        <ul class="navbar-nav mr-auto">
          <li class="nav-item">
            <router-link class="nav-link" to="/"> Home </router-link>
          </li>
          <li class="nav-item">
            <router-link class="nav-link" to="/foods"> Foods </router-link>
          </li>
        </ul>

        <ul class="navbar-nav ml-auto">
          <li class="nav-item">
            <router-link class="nav-link" to="/keranjang">
              Keranjang
              <b-icon-bag></b-icon-bag>
              <span class="badge badge-success ml-2">{{
                jumlahPesanan.length
              }}</span>
            </router-link>
          </li>
        </ul>
      </div>
    </div>
  </nav>
</template>

<script>
import axios from "axios";

export default {
  name: "Navbar",
  data() {
    return {
      jumlahPesanan: [],
    };
  },
  methods: {
    getJumlahPemesanan(data) {
      this.jumlahPesanan = data;
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/keranjangs")
      .then((response) => {
        // handle success
        this.getJumlahPemesanan(response.data);
      })
      .catch((error) => {
        // handle error
        console.log(error, "error");
      });
  },
};
</script>

<style></style>
