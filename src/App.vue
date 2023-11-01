<template>
  <v-app>

    <navbarComp></navbarComp>
    <v-main>
      <router-view />
    </v-main>
    <footerComp></footerComp>
  </v-app>
</template>

<script>
import navbarComp from './components/NavBar.vue'
import footerComp from './components/FooterBar.vue'

export default {
  name: 'App',
  components: {
    navbarComp,
    footerComp
  },

  data: () => ({
    //
  }),
};
</script>

<style>
#app {
  background-size: auto;
  background-color: rgb(235, 235, 235);
}
</style>
