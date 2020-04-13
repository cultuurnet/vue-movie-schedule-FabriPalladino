<template>
  <section id="app">
    <img alt="publiq logo" class="logo" src="./assets/logo.png">
    <h1>Movie Schedule</h1>

    <DateInterval />
    <Schedule />

  </section>
</template>

<script>
import Schedule from "@/components/Schedule";
import DateInterval from "@/components/DateInterval";


export default {
  name: 'App',
  components: {
    Schedule,
    DateInterval
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
  margin-top: 50px;
}
.logo{
  width: 100px;
}
</style>
