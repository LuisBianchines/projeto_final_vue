<template>
  <Banner />
  <div class="main-container">
    <h1>Gerenciar Pedidos</h1>
    <Dashboard />
  </div>
</template>

<script>
import Banner from '@/components/Banner.vue';
import Dashboard from '@/components/Dashboard.vue';

export default {
    name: "Pedidos",
    components: {
    Banner,
    Dashboard
}
}
</script>

<style scoped>

</style>
