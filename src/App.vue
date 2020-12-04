<template>
  <div id="app"> 
    <router-view @authenti="setAuthenti" />
    <Footer/>
  </div>
</template>

<script>
import Footer from '@/components/Footer.vue'

export default {
  components: {
    Footer
  },
  data() {
    return {
      authenti: false
    }
  },
  mounted() {
     if (!this.authenticated) {
      this.$router.replace({ name: "Login" });
    }
  },
  methods: {
    setAuthenti(status) {
      this.authenticated = status;
    }
  },
}
</script>

