<template>
  <div id="app">
    <!-- <img src="./assets/logo.png"> -->


    <Myheader></Myheader>

    <Mynav></Mynav>


    <!-- 路由出口 -->
    <!-- 路由匹配到的组件将渲染在这里 -->
    <router-view></router-view>

  </div>
</template>

<script>

import Myheader from './components/header/Header.vue'
import Mynav from './components/Nav/Nav.vue'

export default {
  name: 'App',
  components: {
    Myheader,Mynav
  }
}
</script>

<style>

</style>
