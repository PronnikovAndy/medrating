<template>
  <Header
    :tabs="tabs"
    @change-route="changeRoute"
  />
  <main>
    <component :is="currentTabComponent"></component>
  </main>
</template>

<script>
import Header from './Header';
import Catalog from './Catalog/index';
import Favourites from "./Favourites";

export default {
  data() {
    return {
      currentTab: 'Catalog',
      tabs: ['Catalog', 'Favourites']
    }
  },
  computed: {
    currentTabComponent: function () {
      return this.currentTab;
    }
  },
  methods: {
    changeRoute(route) {
      this.currentTab = route;
    }
  },
  components: {
    Header,
    Catalog,
    Favourites
  }
}
</script>