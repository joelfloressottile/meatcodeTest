<template>
  <div id="app">
    <router-view />
  </div>
</template>

<script>
export default {
  name: "App"
};
</script>

<style>
#app {
  font-family: "", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

@font-face {
  font-family: "Open Sans";
  src: local("OpenSans"),
    url(./assets/fonts/OpenSans/OpenSans-Regular.ttf) format("truetype");
}

@font-face {
  font-family: "Caveat";
  src: local("Caveat"),
    url(./assets/fonts/Caveat/Caveat-Regular.ttf) format("truetype");
}
</style>
