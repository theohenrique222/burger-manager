<template>
  <div class="main-container">
    <Banner />
    <h1>Monte o seu hamburger:</h1>
    <BurgerForm />
  </div>  
</template>

<script>
  import BurgerForm from "../components/BurgerForm.vue";
  import Banner from "../components/Banner.vue";

  export default {
    name: "Home",
    components: {
      Banner,
      BurgerForm,
    }
  }
</script>
<style scoped>
  * {
    margin: 0;
  }
</style>
