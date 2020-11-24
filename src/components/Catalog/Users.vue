<template>
  <ul>
    <li
        @click.prevent="getAlbums(item.id)"
        v-for="item of items"
        :key="item.id"
    >
      <span
          :class="[albums.length && (albums[0].userId === item.id) ? 'arrow-down' : 'arrow-right']"
      >{{ item.name }}</span>
      <Albums
          v-if="albums.length && (albums[0].userId === item.id)"
          :items="albums"
      />
    </li>
  </ul>
</template>

<script>
import Albums from "./Albums";

export default {
  props: ['items'],
  data() {
    return {
      albums: [],
      loading: false
    }
  },
  methods: {
    getAlbums(userId) {
      if (!this.albums.length) {
        fetch(`https://json.medrating.org/albums?userId=${userId}`)
            .then(response => response.json())
            .then(data => {
              this.albums = data;
              this.loading = false;
            })
      } else {
        this.albums = [];
        this.loading = false;
      }
    },
  },
  components: {
    Albums
  }
}
</script>