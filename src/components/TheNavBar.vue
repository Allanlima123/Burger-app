<template>
  <header>
    <nav id="nav">
      <router-link to="/" id="logo-url">
        <img :src="logo" id="logo" :alt="alt"/> 
      </router-link> 
  
      <router-link to="/">Home</router-link> 
      <span>|</span>
      <router-link to="/Pedidos">Pedidos</router-link>
      <span>|</span>
      <router-link to="/Contato">Contato</router-link>
    </nav>
  </header>
</template>
  
<script>
export default {
  name: "TheNavBar",

  props : ["logo", 'alt']
};
</script>
  
<style scoped>
  #nav{
    background: #222;
    border-bottom: 4px solid #111;
    padding: 15px 50px;
    display: flex;
    justify-content: flex-end;
    align-items: center;
    position: fixed;
    z-index: 99;
    right: 0;
    left: 0;
    top: 0;
  }

  #nav #logo-url{
    margin: auto;
    margin-left: 0;
  }

  #logo{
    width: 34px;
    height: 34px;
  }

  #nav a{
    color: #FCBA03;
    text-decoration: none;
    margin: 12px;
    transition: .5s;
  }

  #nav a:hover{
    color: #fff;
  }

  span{
    color: #fff;
  }
</style>