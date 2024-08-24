<template>
  <div class="main-container">
    <Banner />
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
    .form {
      margin: 20px;
    }
  }
</style>
