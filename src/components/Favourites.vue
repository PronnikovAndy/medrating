<template>
  <ul>
    <li
      v-for="favourite of favourites"
      :key="favourite.id"
    >
      <img :src="favourite.thumbnailUrl" @click="showImageModal(favourite.url)">
      <p>{{ favourite.title }}</p>
    </li>
  </ul>
  <ImageModal
      @hide-modal="hideImageModal"
      :previewUrl="previewUrl"
  />
</template>

<script>
import ImageModal from "@/components/ImageModal";

export default {
  data() {
    return {
      previewUrl: null,
      favourites: JSON.parse(localStorage.getItem("favourites"))
    }
  },
  methods: {
    showImageModal(url) {
      this.previewUrl = url;
    },
    hideImageModal() {
      this.previewUrl = null;
    }
  },
  components: {
    ImageModal
  }
}
</script>

<style>
img:hover {
  cursor: pointer;
}
</style>