<template>
  <ul>
    <li
        @click.stop.prevent="getPhotos(item.id)"
        v-for="item of items"
        :key="item.id"
    >
      <span
          :class="[photos.length && (photos[0].albumId === item.id) ? 'arrow-down' : 'arrow-right']"
      >{{ item.title }}</span>
      <Photos
          v-if="photos.length && (photos[0].albumId === item.id)"
          :items="photos"
      />
    </li>
  </ul>
</template>

<script>
import Photos from "./Photos";

export default {
  props: ['items', 'userId'],
  data() {
    return {
      photos: []
    }
  },
  methods: {
    getPhotos(albumId) {
      if (!this.photos.length) {
        fetch(`https://json.medrating.org/photos?albumId=${albumId}`)
            .then(response => response.json())
            .then(data => this.photos = data);
      } else {
        this.photos = [];
      }
    },
  },
  components: {
    Photos
  }
}
</script>