<template>
  <div id="app">
    <BestTransport/>
    <FormTransportPage/>
  </div>
</template>

<script>
import BestTransport from './components/BestTransport.vue'
import FormTransportPage from './components/FormTransportPage.vue';

export default {
  name: 'App',
  components: {
    BestTransport,
    FormTransportPage
  }
}
</script>

<style>
</style>
