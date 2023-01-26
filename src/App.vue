<template>
  <img alt="Vue logo" src="./assets/logo.png">
  <typeGame />
</template>

<script>
import typeGame from './components/typeGame.vue'

export default {
  name: 'App',
  components: {
    typeGame
  }
}
</script>

<style>
img{
  width: 200px;
  height: 200px;
  margin:10px auto;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
