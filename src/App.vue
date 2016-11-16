<template>
  <div id="app">
    <hex-calendar></hex-calendar>
    <hex-calendar :controls="false"></hex-calendar>
    <hex-calendar controls="month"></hex-calendar>
    <hex-calendar controls="year"></hex-calendar>
  </div>
</template>

<script>
  import HexCalendar from './components/HexCalendar.vue'

  export default {
    name: 'app',
    components: {
      HexCalendar
    }
  }
</script>

<style>
  body {
    font-family: Helvetica, sans-serif;
  }
</style>
