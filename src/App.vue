<template>
  <div id="app">
    <h1 class="title">Test technique : Crée un datepicker</h1>
    <DatePicker />
  </div>
</template>

<script>
  import DatePicker from './components/DatePicker.vue'

  export default {
    name: 'App',
    components: {
      DatePicker
    }
  }
</script>

<style>
  #app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }

  .title {
    margin-bottom: 1em;
  }
</style>