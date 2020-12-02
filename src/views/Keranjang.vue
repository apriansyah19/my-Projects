<template>
  <div class="keranjang">
    <Navbar :keranjangs ="keranjangs"/>
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
                    <button class="btn btn-link" @click="deleteFood(keranjang.id)">
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
    };
  },
  methods: {
    setKeranjang(data) {
      this.keranjangs = data;
      console.log(this.keranjangs, 'datanya');
    },
    convertToRupiah(angka) {
      let rupiah = "";
      let changeString = '' + angka
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