<template>
  <div id="app">
    <scale>
      <div class="content">
        Content
      </div>
    </scale>
  </div>
</template>

<script>
import Scale from './components/VueScale'

export default {
  name: 'app',
  components: {
    Scale
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  width: 600px;
  height: 400px;
  margin: 60px auto;
}
.content{
  font-size: 60px;
  margin-top: 170px;
}
</style>
