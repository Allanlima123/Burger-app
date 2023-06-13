<template>
  <main class="main--container">
    <h1>Gerenciar Pedidos: </h1>
    <dashboard />
  </main>
</template>

<script>
import Dashboard from '../components/Dashboard.vue'

export default {
  name : 'Pedidos',

  components : {
    Dashboard
  }
  
}
</script>

<style scoped>
h1{
  margin-top: 110px;
}
</style>
