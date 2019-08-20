<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <img class="iota-logo" alt="IOTA logo" src="./assets/iota_logo.png">

    <HelloWorld />
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'app',
  components: {
    HelloWorld
  }
}
</script>

<style>
body {
  margin: 0;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.iota-logo {
  width: 200px;
}
</style>
