<template>
  <nav id="nav">
    <router-link to="/" id="logo-url">
        <img :src="logo" :alt="alt" id="logo">
    </router-link>
    <router-link to="/">Inicio</router-link>
    <router-link to="/solicitations">Pedidos</router-link>
  </nav>
</template>

<script>
export default {
    name: "Navbar",
    props: [
      "logo",
      "alt"
    ]
}
</script>

<style>
  #nav {
    background-color: #222;
    border-bottom: 4px solid #111;
    padding: 2px 40px;
    display: flex;
    justify-content: flex-end;
    align-items: center;
  }
  #nav #logo-url {
    margin: auto;
    margin-left: 0;
  }
  #logo {
    width: 60px;
    height: 60px;
  }
  #nav a{
    color: #FCBA03;
    text-decoration: none;
    margin: 12px;
    transition: .5s;
  }
  #nav a:hover {
    color: #FFF;
  }
</style>