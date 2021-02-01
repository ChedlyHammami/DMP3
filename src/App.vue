<template>

  <div id="app" >
    <Logo/>
    <Search/>
  </div>
</template>

<script>

import Logo from './components/Logo';
import Search from './components/Search';

export default {
  name: 'App',
  components: {
    Logo,
    Search
  }
  ,data(){
    return {
      ch:false
    }
  }
}
</script>

<style>

</style>
