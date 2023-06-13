<template>
  <TheNavBar :logo="logo_src" :alt="app_name"/>
  <router-view />
  <TheFooter />
</template>

<script>
import TheNavBar from './components/TheNavBar.vue';
import TheFooter from './components/TheFooter.vue';

export default {
  name : "App",

  data(){
    return{
      logo_src : '/img/logo.png',
      app_name : 'Make Your Burger'
    }
  },
  components : {
    TheNavBar,
    TheFooter
  }
}
</script>

<style>
body::-webkit-scrollbar {
    width: 15px;            /* largura da barra de rolagem */
  }
  
body::-webkit-scrollbar-thumb {
    background-color: #fcda03;
     /* cor do scroll (thumb) da barra de rolagem */
    /* arredondar o scroll (thumb) */          
    /* criar um espaco em volta do scroll */
}

body{
  max-width: 1440px;
  margin: 0 auto;
}

*{
  font-family: Arial, Helvetica, sans-serif;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  outline: none;
}

.main--container{
  margin: 50px;
  min-height: 40vh;
}

h1{
  text-align: center;
  font-size: 42px;
  margin-bottom: 30px;
  color: #222;
}
</style>

