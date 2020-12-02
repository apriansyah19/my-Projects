<template>
  <div class="card shadow card-product">
    <img
      :src="'assets/images/' + cardProduct.gambar"
      class="card-img-top"
      width="100%"
      height="230px"
    />
    <div class="card-body">
      <h5 class="card-title">{{ cardProduct.nama }}</h5>
      <p class="card-text">Harga : Rp {{ convertToRupiah(cardProduct.harga) }}</p>
      <router-link :to="'/foods/' + cardProduct.id" class="btn btn-success"
        ><b-icon-cart></b-icon-cart> Pesan</router-link
      >
    </div>
  </div>
</template>

<script>

export default {
  name: "Products",
  props: ["cardProduct"],
  methods: {
    convertToRupiah(angka) {
      let rupiah = "";
      let angkarev = angka.toString().split("").reverse().join("");
      for (let i = 0; i < angkarev.length; i++)
        if (i % 3 == 0) rupiah += angkarev.substr(i, 3) + ".";
      return (
        rupiah
          .split("", rupiah.length - 1)
          .reverse()
          .join("")
      );
    },
  },
};
</script>

<style>
</style>