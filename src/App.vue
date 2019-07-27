<template>
  <div id="app">
    <Calculator />
  </div>
</template>

<script>
import Calculator from './components/Calculator.vue'

export default {
  name: 'app',
  components: {
    Calculator
  }
}
</script>

<style lang="scss">
@import './scss/_mixin.scss';
* {
  box-sizing: border-box;
}
body {
  margin: 0;
  height: 100vh;
  min-height: 600px;
  background-color: $bgGray;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  height: 100%;
}
</style>
