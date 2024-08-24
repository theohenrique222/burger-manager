<template>
  <footer id="footer">
    <p>Burger Manager &copy; 2024</p>
  </footer>
</template>

<script>
export default {
    name: "Footer"
}
</script>

<style scoped>
  #footer {
    bottom: 0;
    height: 200px;
    background-color: #222;
    border-top: 4px solid #111;
    color: #FCBA03;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
  } 
  @media (max-width: 400px) {
    #footer {
      height: 120px;
      font-size: .9rem;
    }
  }
</style>