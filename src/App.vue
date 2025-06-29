<template>
  <div id="app">
    <ProductDisplay />
  </div>
</template>

<script>
import ProductDisplay from './components/ProductDisplay.vue'

export default {
  name: 'App',
  components: {
    ProductDisplay
  }
}
</script>

<style>
body {
  font-family: sans-serif;
  background-color: #f9f9f9;
  margin: 0;
  padding: 0;
}
</style>
