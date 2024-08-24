<template>
  <div id="main-banner" class="flex items-center bg-no-repeat bg-cover">
    <h1 class="text-yellow-500 text-center bg-neutral-900 py-5 px-10 font-bold">Burger Manager</h1>
  </div>
</template>

<style scoped>
    #main-banner {
        background-image: url("../../public/img/background.png");
        background-position: 0 -250px;
        height: 500px;
    }
    /* #main-banner h1{
      color: #FFF;
      text-align: center;
      background-color: #222;
      padding: 20px 40px;
      
    } */
    h1 {
      font-size: 1.5rem;
    }
    @media (max-width: 1000px) {
      #main-banner {
        background-position: center;
      }
    }
</style>

<script>
export default {
    name: "Banner",
}
</script>
