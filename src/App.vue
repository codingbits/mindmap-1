<template>
  <div id="app">
    <Editor></Editor>
  </div>
</template>

<script>
import Editor from './editor/editor'

export default {
  name: 'app',
  components: {
    Editor
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
}
</style>
