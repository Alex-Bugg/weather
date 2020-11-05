<template>
  <div id="app">
    <router-view/>
    <AppWeather />
  </div>
</template>

<script>
import './assets/normalize.css'
import AppWeather from './components/App-weather'
export default {
  components: {
    AppWeather
  }
}
</script>

<style lang="scss">
#app {
  margin: 0 auto;
  max-width: 650px;
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}
body {
  background: rgb(0,164,229);
  background: linear-gradient(354deg, rgba(0,164,229,1) 0%, rgba(255,145,145,1) 100%);
  background-repeat: no-repeat;
  background-size: cover;
  min-height: 100vh;
  padding-top: 50px;
}
</style>
