<template>
  <div id="app">
    <Upload/>
  </div>
</template>

<script>
import Upload from './components/Upload.vue'

export default {
  name: 'App',
  components: {
    Upload
  }
}
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Poppins&display=swap');
#app {
  font-family: 'Poppins', sans-serif;
  margin-top: 60px;
}
</style>
