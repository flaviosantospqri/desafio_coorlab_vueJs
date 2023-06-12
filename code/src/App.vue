<template>
  <div id="app">
    <BestTransport/>
    <div class="container-body-page">
      <FormTransportPage/>
    </div>
  </div>
</template>

<script>
import BestTransport from './components/BestTransport.vue'
import FormTransportPage from './components/FormTransportPage.vue';

export default {
  name: 'App',
  components: {
    BestTransport,
    FormTransportPage,
  }, 
}
</script>

<style>
    .container-body-page{
      display: flex;
      max-width: 1100px;
      align-items: center;
      justify-content: space-evenly;
      background-color: #F8F9FA;
      margin: 0 auto;
      flex-wrap: wrap;
      margin-top: 5%;
      border-radius: 8px;
      box-shadow: -5px 2px 18px -6px rgba(0,0,0,0.75);
;
    }
</style>
