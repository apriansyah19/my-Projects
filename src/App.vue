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
     if (!this.authenti) {
      this.$router.push({ path: "/login"})
    }
  },
  methods: {
    setAuthenti(status) {
      this.authenti = status;
    }
  },
}
</script>

