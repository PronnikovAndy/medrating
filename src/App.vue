<template>
  <Header
    @change-route="changeRoute"
  />
  <Main
    v-if="route === 'catalog'"
  />
  <Favourites
    v-if="route === 'favourites'"
  />
</template>

<script>
import Header from '@/components/Header';
import Main from '@/components/Main';
import Favourites from '@/components/Favourites';

export default {
  name: 'App',
  data() {
    return {
      route: 'catalog',
    }
  },
  components: {
    Header,
    Main,
    Favourites
  },
  methods: {
    changeRoute(route) {
      this.route = route;
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
