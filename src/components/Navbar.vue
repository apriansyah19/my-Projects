<template>
 <b-navbar toggleable="lg" variant="light">
    <div class="container">
      <b-navbar-brand href="#">Satria Resto</b-navbar-brand>

      <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

      <b-collapse id="nav-collapse" is-nav>
        <b-navbar-nav>
          <router-link class="nav-link" to="/"> Home </router-link>
          <router-link class="nav-link" to="/foods"> Foods </router-link>
        </b-navbar-nav>

        <!-- Right aligned nav items -->
        <b-navbar-nav class="ml-auto">
          <router-link class="nav-link" to="/login"> <button type="button" class="btn btn-dark btn-sm">Logout</button></router-link>
          <router-link class="nav-link" to="/keranjang">
            Keranjang
            <b-icon-bag></b-icon-bag>
            <span class="badge badge-success ml-2">{{
              keranjangs ? keranjangs.length : jumlahPesanan.length
            }}</span>
          </router-link>
        </b-navbar-nav>
      </b-collapse>
    </div>
  </b-navbar>
</template>

<script>
import axios from "axios";

export default {
  name: "Navbar",
  props: ["keranjangs"],
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
