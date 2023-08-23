<template>
  <banner />
  <div class="main-container">
    <h1>Monte seu Burger</h1>
    <burger-form />
  </div>

</template>
<script>
  import BurgerForm from '@/components/BurgerForm.vue';
  import Banner from '../components/Banner.vue';
  
  export default {
    components: {
      Banner,
      BurgerForm
    }
  }
</script>
