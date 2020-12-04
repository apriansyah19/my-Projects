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
                  <input v-model="username" type="text" class="form-control" />
                </div>
                <div class="form-group">
                  <label for="password">Password</label>
                  <input
                    v-model="password"
                    type="password"
                    class="form-control"
                  />
                </div>
                <button
                  @click="login()"
                  type="submit"
                  class="btn btn-success mr-2"
                >
                  Login
                </button>
                <button
                  @click="register"
                  type="submit"
                  class="btn btn-secondary"
                >
                  Register
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
  name: "Login",
  data() {
    return {
      username: "",
      password: "",
    };
  },
  methods: {
    login() {
      console.log(this.username, this.password, "isi data");
      axios
        .get("http://localhost:3000/dataLogin?username=" + this.username)
        .then((response) => {
          if (response.data.length !== 0) {
            for (let i = 0; i < response.data.length; i++) {
              if (response.data[i].password === this.password) {
                this.$emit("authenti", true);
                this.$router.push({ path: "/" });
              } else {
                this.$toast.error("Username dan Password anda salah", {
                  type: "error",
                  position: "top-right",
                  duration: 2000,
                  dismissible: true,
                });
              }
            }
          } else {
            this.$toast.error("Username dan Password anda salah", {
              type: "error",
              position: "top-right",
              duration: 2000,
              dismissible: true,
            });
          }
          // handle success
        })
        .catch((error) => {
          // handle error
          console.log(error, "error");
        });
    },
    register() {
      this.$router.push({ path: "/register" });
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