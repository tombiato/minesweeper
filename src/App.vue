<template>
  <div id="app">
    <h1>CodeBuds Minesweeper</h1>
    <grid />
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
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-family: 'Montserrat', sans-serif;
  background-color: #46bedd;
  color: #46bedd;
  font-weight: bold;
  font-size: 1.5em;
  text-align: center;
}
* {
  margin: 0 auto;
}

h1 {
  color: #ffda02;
  margin-bottom: 100px;
}
</style>
