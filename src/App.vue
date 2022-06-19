<!--
 * @Description: 
 * @version: 1.0.0
 * @Author: changhaotian6@163.com
 * @Date: 2022-06-19 11:21:58
 * @LastEditors: changhaotian6@163.com
 * @LastEditTime: 2022-06-19 16:46:45
 * @FilePath: \openlayers-demo\src\App.vue
-->
<template>
  <div id="app">
    <OpenLayers/>
  </div>
</template>

<script>
import OpenLayers from './components/openlayers/index.vue'

export default {
  name: 'App',
  components: {
    OpenLayers
  }
}
</script>

<style lang="scss">
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    list-style: none;
}

</style>
