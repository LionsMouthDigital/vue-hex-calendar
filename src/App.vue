<template>
  <div id="app">
    <div style="width: 200px">
      <hex-calendar></hex-calendar>
    </div>

    <div style="width: 200px">
      <hex-calendar :controls="false"></hex-calendar>
    </div>

    <div style="width: 200px">
      <hex-calendar controls="month"></hex-calendar>
    </div>

    <div style="width: 200px">
      <hex-calendar controls="year"></hex-calendar>
    </div>
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
  * + * {
    margin-top: 20px;
  }

  body {
    font-family: Helvetica, sans-serif;
  }

  .calendar .controls {
    display: flex;
    justify-content: center;
  }

  .calendar .controls div {
    display: flex;
    flex-basis: 50%;
    justify-content: space-between;
    padding-left: 5px;
    padding-right: 5px;
  }

  .calendar .controls div span {
    text-align: center;
  }

  .calendar-header {
    display: flex;
    flex-direction: column;
  }

  .days-of-week {
    font-weight: bold;
  }

  .days-of-week,
  .week {
    display: flex;
    margin-top: 0;
  }

  .days-of-week div,
  .day {
    flex-basis: calc(7 / 12 * 100%);
    padding: 5px;
    text-align: center;
  }

  .calendar .controls div,
  .calendar .controls div * + *,
  .days-of-week div,
  .day {
    margin-top: 0;
  }

  .sr-only {
    border: 0;
    clip: rect(0, 0, 0, 0);
    height: 1px;
    margin: -1px;
    overflow: hidden;
    padding: 0;
    position: absolute;
    width: 1px;
  }
</style>
