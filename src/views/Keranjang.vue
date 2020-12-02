<template>
  <div class="keranjang">
    <Navbar :keranjangs="keranjangs" />
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
                Keranjang
              </li>
            </ol>
          </nav>
        </div>
      </div>

      <div class="row">
        <div class="col">
          <h2>Keranjang <strong>Saya</strong></h2>
          <div class="table-responsive mt-3">
            <table class="table">
              <thead class="thead-light">
                <tr>
                  <th scope="col">No</th>
                  <th scope="col">Foto</th>
                  <th scope="col">Makanan</th>
                  <th scope="col">Keterangan</th>
                  <th scope="col">Jumlah</th>
                  <th scope="col">Harga</th>
                  <th scope="col">Total Harga</th>
                  <th scope="col">Hapus</th>
                </tr>
              </thead>
              <tbody>
                <tr
                  v-for="(keranjang, index) in keranjangs"
                  :key="keranjang.id"
                >
                  <th>{{ index + 1 }}</th>
                  <td>
                    <img
                      :src="'../assets/images/' + keranjang.productsTemp.gambar"
                      class="img-fluid shadow"
                      width="200px"
                      height="100px"
                    />
                  </td>
                  <td>
                    <strong>{{ keranjang.productsTemp.nama }}</strong>
                  </td>
                  <td>
                    {{ keranjang.keterangan ? keranjang.keterangan : "-" }}
                  </td>
                  <td>{{ keranjang.jumlah_pemesanan }}</td>
                  <td>
                    Rp. {{ convertToRupiah(keranjang.productsTemp.harga) }}
                  </td>
                  <td align="right">
                    <strong
                      >Rp. {{ convertToRupiah(keranjang.total_harga) }}</strong
                    >
                  </td>
                  <td align="center" class="text-danger">
                    <button
                      class="btn btn-link"
                      @click="deleteFood(keranjang.id)"
                    >
                      <b-icon-trash2 class="text-danger"> </b-icon-trash2>
                    </button>
                  </td>
                </tr>
                <tr>
                  <td colspan="6" align="right">
                    <strong>Total Harga</strong>
                  </td>
                  <td align="right">
                    <strong>Rp. {{ convertToRupiah(sumHarga) }}</strong>
                  </td>
                  <td></td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>

      <!-- form pemesanan -->
      <div class="row justify-content-end">
        <div class="col-md-4">
          <form class="mt-4" v-on:submit.prevent>
            <div class="form-group">
              <label for="nama">Nama :</label>
              <input type="text" class="form-control" v-model="pesanan.nama" />
            </div>
            <div class="form-group">
              <label for="no_meja">No Meja :</label>
              <input
                type="number"
                class="form-control"
                v-model="pesanan.no_meja"
              />
            </div>
            <button
              type="submit"
              class="btn btn-success mt-3"
              @click="checkOut"
            >
              <b-icon-cart></b-icon-cart> Pesan
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
  name: "Keranjang",
  components: {
    Navbar,
  },
  data() {
    return {
      keranjangs: [],
      pesanan: {},
    };
  },
  methods: {
    checkOut() {
      if (this.keranjangs.length !== 0) {
        if (this.pesanan.nama && this.pesanan.no_meja) {
        this.pesanan.keranjangs = this.keranjangs;
        axios.post("http://localhost:3000/pesanans", this.pesanan).then(() => {
          this.keranjangs.map(function (item) {
            return axios
              .delete("http://localhost:3000/keranjangs/" + item.id)
              .catch((error) => {
                // handle error
                console.log(error, "error");
              });
          });
          this.getDataFood()
          this.$router.push({ path: "/pesanan-success" });
          this.$toast.success("Pesanan Anda Telah di Proses, Terimakasih.", {
            type: "success",
            position: "top-right",
            duration: 2000,
            dismissible: true,
          });
        });
      } else {
        this.$toast.error("Nama dan No Meja Harus diisi.", {
          type: "error",
          position: "top-right",
          duration: 2000,
          dismissible: true,
        });
      }
      } else {
        this.$toast.error("Keranjang Anda Kosong", {
          type: "error",
          position: "top-right",
          duration: 2000,
          dismissible: true,
        });
      }
      
    },
    setKeranjang(data) {
      this.keranjangs = data;
      console.log(this.keranjangs, "datanya");
    },
    convertToRupiah(angka) {
      let rupiah = "";
      let changeString = "" + angka;
      let angkarev = changeString.split("").reverse().join("");
      for (let i = 0; i < angkarev.length; i++)
        if (i % 3 == 0) rupiah += angkarev.substr(i, 3) + ".";
      return rupiah
        .split("", rupiah.length - 1)
        .reverse()
        .join("");
    },
    getDataFood() {
      axios
        .get("http://localhost:3000/keranjangs")
        .then((response) => {
          // handle success
          this.setKeranjang(response.data);
        })
        .catch((error) => {
          // handle error
          console.log(error, "error");
        });
    },
    deleteFood(data) {
      axios
        .delete("http://localhost:3000/keranjangs/" + data)
        .then((response) => {
          // handle success
          console.log(response, "response");
          this.getDataFood();
        })
        .catch((error) => {
          // handle error
          console.log(error, "error");
        });
    },
  },
  mounted() {
    this.getDataFood();
  },
  computed: {
    sumHarga() {
      return this.keranjangs.reduce(function (item, data) {
        return item + data.total_harga;
      }, 0);
    },
  },
};
</script>

<style>
</style>