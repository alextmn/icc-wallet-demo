<template>
<div style="display: flex; justify-content: space-between;">
  <div>&nbsp;</div>
  <div style="font-size: 30px; align-self: flex-end;">ICC-Solana (QNT) Wallet Demo</div>
  <img alt="Vue logo" src="./assets/qnt.png" style="wight:100px; height:50px; align-self: right;" />
</div>
  <HelloWorld msg=""/>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: black;
  margin-top: 60px;
  min-height: 400px;
  background-image: url('assets/background.png') ;
}
</style>
