<template>
  <header>
    <a
        href="#"
        v-for="tab of tabs"
        @click.prevent="$emit('change-route', tab)"
        :key="tab"
    >{{ tab }}</a>
  </header>
</template>

<script>
export default {
  props: ['tabs']
}
</script>

<style>
header {
  display: flex;
  justify-content: center;
  align-items: center;
}

a {
  margin: 0 10px;
}
</style>