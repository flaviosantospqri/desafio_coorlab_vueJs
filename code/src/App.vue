<template>
  <div id="app">
    <BestTransport/>
    <div class="container-body-page">
      <FormTransportPage/>
      <ItemCard message="Frete com menor valor" tranpostName="Sedex" leadTime="2" price="2.5"/>
    </div>
   
  </div>
</template>

<script>
import BestTransport from './components/BestTransport.vue'
import FormTransportPage from './components/FormTransportPage.vue';
import ItemCard from './components/ItemCard.vue';

export default {
  name: 'App',
  components: {
    BestTransport,
    FormTransportPage,
    ItemCard
  }
}
</script>

<style>
    .container-body-page{
      display: flex;
      align-items: center;
      justify-content: space-evenly;
      background-color: #F8F9FA;
      margin: 30px;
      border-radius: 8px;
      box-shadow: -5px 2px 18px -6px rgba(0,0,0,0.75);
;
    }
</style>
