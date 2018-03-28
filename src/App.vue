<template>
  <div id="app">
    <!--router-view/-->
    <Projects/>
    <Destination/>
  </div>
</template>

<script>
import Projects from    './components/Projects';
import Destination from './components/Destination';

export default {
  name: 'App',
  components: {
   Projects,
   Destination
   
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
  margin-top: 60px;
}
</style>
