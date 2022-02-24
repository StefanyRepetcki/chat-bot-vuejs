<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}

body {
    background: rgb(2, 48, 97);
    background: linear-gradient(90deg, rgb(14, 57, 104) 0%, rgb(43, 99, 158) 35%, rgb(214, 164, 35) 100%);
}

#nav {
  padding: 30px;
}

#nav a {
  font-weight: bold;
  color: #0f5091;
}

#nav a.router-link-exact-active {
  color: #d1c51f;
}
</style>
