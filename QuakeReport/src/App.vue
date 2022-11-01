<template>

 
  <HelloWorld msg="QUAKEREPORT"/>
  <quakelist />
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import quakelist from './components/quakelist.vue'

export default {
  name: 'App',
  components: {
    HelloWorld,
    quakelist
  }
}
</script>

<style>

*{
  box-sizing: border-box;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
 
  color: #2c3e50;
  margin:0;
  padding:0;
}
html ,body{
  margin:0;
  padding:0;
width:100%;
}

</style>
