<template>
  <div id="app">
    <quote/>
  </div>
</template>

<script>
import quote from './components/quote';

export default {
  name: 'App',
  components:{
    quote
  },
  data:()=>{
    return{

    }
  }

}
</script>

<style>
#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100vh;
}
</style>
