<template>
  <div id="main-banner" style="background-image: url('/img/background.png');">
    <img src="" alt="">
    <div class="main-banner">
    </div>
    <h1>Burger Manager</h1>
  </div>
</template>

<script>
export default {
    name: "Banner",
}
</script>

<style scoped>
    #main-banner {
        background-position: 0 -250px;
        background-size: cover;
        height: 500px;
        display: flex;
        align-items: center;
        justify-content: flex-start;
        margin-bottom: 20px;
    }
    #main-banner h1{
      color: #FFF;
      text-align: center;
      background-color: #222;
      padding: 20px 40px;
      
    }
</style>