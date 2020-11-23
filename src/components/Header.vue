<template>
  <header>
    <a href="/catalog" @click.prevent="$emit('change-route', 'catalog')">Catalog</a>
    <a href="/favourites" @click.prevent="$emit('change-route', 'favourites')">Favourites</a>
  </header>
</template>

<style>
header {
  display: flex;
  justify-content: center;
  align-items: center;
}

img {
  width: 40px;
  height: 40px;
}

a {
  margin: 0 10px;
}
</style>