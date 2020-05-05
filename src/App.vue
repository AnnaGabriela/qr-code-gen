<template>
  <div id="app">
    <QRCodeGen />
  </div>
</template>

<script>
import QRCodeGen from './components/QRCodeGen.vue';

export default {
  name: 'App',
  components: {
    QRCodeGen,
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #1c1d1f;
  width: 100%;
  height: 100vh;
  background: url("./assets/images/bg.png") no-repeat center center fixed;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
}

body {
  width: 100%;
  margin: 0;
  padding: 0;
}
</style>
