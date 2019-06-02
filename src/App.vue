<template>
  <div id="app">
    <files-vue></files-vue>
  </div>
</template>

<script>
import FilesVue from './components/FilesView.vue'

export default {
  name: 'app',
  components: {
    FilesVue
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
