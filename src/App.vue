<template>
  <div id="app">
    <h1>CodeBuds Minesweeper</h1>
    <grid/>
  </div>
</template>

<script>
import Grid from "@/components/Grid";

export default {
  name: 'App',
  components: {
    Grid
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat&display=swap');
body {
  font-family: 'Montserrat', sans-serif;
  background-color: #46bedd;
  color: #ffe54e;
  text-align: center;
}
* {
  margin: 0 auto;
}
</style>
