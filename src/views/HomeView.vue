<template>
  <div class="main-container">
    <Banner />
    <h1>Monte o seu hamburger:</h1>
    <BurgerForm class="form"/>
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
  @media (max-width: 400px) {
    h1 {
      font-size: 1.5rem;
      margin-bottom: 15px;
    }
    .form {
      margin: 20px;
    }
  }
</style>
