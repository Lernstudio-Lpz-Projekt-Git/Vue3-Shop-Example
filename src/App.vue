<template>
  <!-- max Anzahl holen: getAllArtikelCounter -->
  <NavComp
    navtitle="Vue.js Evolution - Von der statischen Webpage zum Vue-Projekt"
  />
  <ShopComp />
</template>

<script>
import NavComp  from "./components/Nav-Comp.vue";
import ShopComp from "./components/Shop-Comp.vue";

export default {
  name: "App",
  components: {
    NavComp,
    ShopComp,
  },
};
</script>


<style>
@import "@/assets/css/style.css";
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>
