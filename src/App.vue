<template>
  <div id="app">
    <router-view />
    <Nav id="nav" />
  </div>
</template>
<script>
import Nav from "../src/components/Nav.vue";

export default {
  components: { Nav },
};
</script>

<style lang="less">
* {
  padding: 0;
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  position: relative;
}
#nav {
  position: absolute;
  right: 2rem;
  top: 2rem;
}
.shadow-box {
  box-shadow: 5px 5px 5px gray;
  border-radius: 0.3rem;
}

ul {
  list-style: none;
}
</style>
