<template>
  <div class="pesanan-success">
    <Navbar />
    <div class="container">
      <div class="row justify-content-center mt-5">
        <div class="col text-center">
          <h2>Terima Kasih!!</h2>
          <img
            src="../assets/images/menunggu.png"
            width="500px"
            class="img-fluid"
            alt="Responsive image"
          />
          <h2>Pesanan Anda Segera Datang</h2>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
export default {
  name: "PesananSuccess",
  components: {
    Navbar,
  },
};
</script>

<style>
</style>