<template>
  <ul>
    <li
      v-for="favourite of favourites"
      :key="favourite.id"
    >
      <img :src="favourite.thumbnailUrl">
      <span>{{ favourite.title }}</span>
    </li>
  </ul>
</template>

<script>
export default {
  data() {
    return {
      favourites: JSON.parse(localStorage.getItem("favourites"))
    }
  }
}
</script>