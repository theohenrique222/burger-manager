<template>
  <div class="main-container">
    <h1>Gerenciador de pedidos:</h1>
    <Dashboard />
  </div>
</template>

<script>
import Dashboard from '../components/Dashboard.vue';
  export default {
    name: "Solicitations",
    components: {
      Dashboard
    }
  }
</script>

<style scoped>
* {
  margin: 0;
}
.main-container{
  height: 80vh;
}
  h1 {
    font-size: 3rem;
  }
  @media (max-width: 400px) {
    h1 {
      font-size: 1.2rem;
      margin: 0;
      width: 100%;
    }
    
  }
</style>
