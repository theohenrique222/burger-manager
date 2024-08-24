<template>
  <div>
    <Navbar :logo="logo_src" :alt="app_name"/>
    <router-view class="content min-h-screen"/>
    <Footer class="footer"/>
  </div>
</template>


<script>
  import Navbar from "./components/Navbar.vue";
  import Footer from "./components/Footer.vue";

  export default {
    components: {
      Navbar,
      Footer
    },
    data() {
      return {
        logo_src: "/img/logo.png",
        app_name: "Burger Manager",
      }
    }
  }
</script>
<style>
  * {
    font-family: Helvetica;
    padding: 0;
    margin: 0;
    box-sizing: border-box;
  }

  .main-container {
    margin: 50px;
    min-height: 250px;
  }
  h1 {
    text-align: center;
    color: #222;
  }
  footer {
    position: relative;
    bottom: 0;
  }
</style>