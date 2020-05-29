<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">百度地图显示固定省级范围</router-link> |
      <router-link to="/about">中国地图</router-link> |
        <router-link to="/area/seven">合并中国省份形成七大区</router-link> |
      <router-link to="/tabledemo">表格合并</router-link>
    </div>
    <router-view/>
  </div>
</template>
<style>
html,body{
  margin: 0;
  padding: 0;
  height: 100%;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  height: 100%;
  overflow: hidden;
}

#nav {
  height: 80px;
  line-height: 80px;
  overflow: hidden;
}

#nav a {
  font-weight: bold;
  color: #2c3e50;
}

#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
