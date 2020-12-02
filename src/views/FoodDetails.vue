<template>
  <div class="food-detail">
    <Navbar />
    <div class="container">
      <div class="row mt-5">
        <div class="col">
          <nav aria-label="breadcrumb">
            <ol class="breadcrumb">
              <li class="breadcrumb-item">
                <router-link to="/" class="text-dark">Home </router-link>
              </li>
              <li class="breadcrumb-item">
                <router-link to="/foods" class="text-dark">Foods </router-link>
              </li>
              <li class="breadcrumb-item active" aria-current="page">
                Food Order
              </li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row mt-3">
        <div class="col-md-6">
          <img :src="'../assets/images/' + products.gambar" class="img-fluid" />
        </div>
        <div class="col-md-6">
          <h2>
            <strong>{{ products.nama }}</strong>
          </h2>
          <h4 class="mt-2">
            Harga : <strong>Rp. {{ convertToRupiah(products.harga) }} </strong>
          </h4>
          <form class="mt-4" v-on:submit.prevent>
            <div class="form-group">
              <label for="jumlah_pemesanan">Jumlah Pesan</label>
              <input
                type="number"
                class="form-control"
                v-model="pesanan.jumlah_pemesanan"
              />
            </div>
            <div class="form-group">
              <label for="keterangan">Keterangan</label>
              <textarea
                v-model="pesanan.keterangan"
                class="form-control"
              ></textarea>
            </div>
            <button
              type="submit"
              class="btn btn-success mt-3"
              @click="pemesanan"
            >
              <b-icon-cart></b-icon-cart>Pesan 
              <strong>Rp. {{ convertToRupiah(sumHarga) }} </strong>
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import axios from "axios";

export default {
  name: "FoodDetails",
  components: {
    Navbar,
  },
  data() {
    return {
      products: {},
      pesanan: {},
    };
  },
  methods: {
     convertToRupiah(angka) {
      let rupiah = "";
      let changeString = '' + angka
      let angkarev = changeString.split("").reverse().join("");
      for (let i = 0; i < angkarev.length; i++)
        if (i % 3 == 0) rupiah += angkarev.substr(i, 3) + ".";
      return (
        rupiah
          .split("", rupiah.length - 1)
          .reverse()
          .join("")
      );
    },
    setProducts(data) {
      this.products = data;
    },
    pemesanan() {
      if (this.pesanan.jumlah_pemesanan) {
        this.pesanan.total_harga = this.sumHarga
        this.pesanan.productsTemp = this.products;
        axios
          .post("http://localhost:3000/keranjangs", this.pesanan)
          .then(() => {
            this.$router.push({ path: "/keranjang" });
            this.$toast.success("Pesanan Anda Telah Masuk di Keranjang.", {
              type: "success",
              position: "top-right",
              duration: 2000,
              dismissible: true,
            });
          })
          .catch((err) => console.log(err, "errorr"));
      } else {
        this.$toast.error("Jumlah Pesanan Harus Diisi.", {
          type: "error",
          position: "top-right",
          duration: 2000,
          dismissible: true,
        });
      }
    },
  },
  mounted() {
    axios
      .get("http://localhost:3000/products/" + this.$route.params.id)
      .then((response) => {
        // handle success
        this.setProducts(response.data);
      })
      .catch((error) => {
        // handle error
        console.log(error, "error");
      });
  },
  computed: {
      sumHarga() {
        if (this.pesanan.jumlah_pemesanan === undefined) {
          return 0
        } else {
          return (+this.pesanan.jumlah_pemesanan * this.products.harga)
        }
      }
  }
};
</script>

<style>
</style>