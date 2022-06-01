<template>
  <Converter/>
</template>

<script>
import Converter from './components/converter.vue'

export default {
  name: 'App',
  components: {
    Converter
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

}

body {
  overflow: hidden;
}

* {
  background-color: #D4D3DC;
  color: #2c3e50;
}
</style>
