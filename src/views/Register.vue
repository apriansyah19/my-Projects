<template>
  <section id="cover" class="min-vh-100">
    <div class="container">
      <div class="row justify-content-center">
        <div class="col-md-5">
          <form v-on:submit.prevent>
            <div class="text-center"><h2>Satria Resto</h2></div>

            <div class="card">
              <div class="card-body">
                <div class="form-group">
                  <label for="username">Username</label>
                  <input
                    v-model="dataRegis.username"
                    type="text"
                    class="form-control"
                  />
                </div>
                <div class="form-group">
                  <label for="nama">Fullname</label>
                  <input
                    v-model="dataRegis.fullname"
                    type="text"
                    class="form-control"
                  />
                </div>
                <div class="form-group">
                  <label for="password">Password</label>
                  <input
                    v-model="dataRegis.password"
                    type="password"
                    class="form-control"
                  />
                </div>
                <div class="form-group">
                  <label for="password">Confirm Password</label>
                  <input
                    v-model="dataRegis.confirmPassword"
                    type="password"
                    class="form-control"
                  />
                </div>
                <button
                  @click="register"
                  type="submit"
                  class="btn btn-secondary mr-2"
                >
                  Register
                </button>
                <button
                  @click="loginPage"
                  type="submit"
                  class="btn btn-success mr-2"
                >
                  Login
                </button>
              </div>
            </div>
          </form>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import axios from "axios";
export default {
  name: "Register",
  data() {
    return {
      dataRegis: {},
    };
  },
  methods: {
    register() {
      if (this.dataRegis.password !== this.dataRegis.confirmPassword) {
        this.$toast.error("Password yang dimasukan berbeda", {
          type: "error",
          position: "top-right",
          duration: 2000,
          dismissible: true,
        });
      } else {
        axios
          .post("http://localhost:3000/dataLogin", this.dataRegis)
          .then(() => {
            this.$router.push({ path: "/login" });
            this.$toast.success("Register Berhasil, Sialakan Login.", {
              type: "success",
              position: "top-right",
              duration: 2000,
              dismissible: true,
            });
          })
          .catch((err) => console.log(err, "errorr"));
      }
    },
    loginPage() {
      this.$router.push({ path: "/login" });
    },
  },
};
</script>

<style>
#cover {
  display: flex;
  align-items: center;
}
</style>