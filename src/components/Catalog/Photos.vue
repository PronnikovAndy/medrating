<template>
  <ul>
    <li
        @click.stop.prevent
        v-for="item of items"
        :key="item.id"
        class="photos"
    >
      <span
          :class="favourites && favourites.some(t => t.id === item.id) ? 'star active' : 'star'"
          @click="addToFavourites(item)"
      ></span>
      <img
          @click="showImageModal(item.url)"
          :alt="item.title"
          :src="item.thumbnailUrl"
      />
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
  props: ['items'],
  data() {
    return {
      photos: [],
      previewUrl: null,
      favourites: JSON.parse(localStorage.getItem("favourites"))
    }
  },
  methods: {
    addToFavourites(photo) {
      if(!this.favourites) {
        localStorage.setItem("favourites", JSON.stringify([photo]));
        this.favourites = [photo];
      } else {
        const filtered = this.favourites.filter(item => {
          return item.id !== photo.id;
        });

        if(filtered.length === this.favourites.length) {
          localStorage.setItem("favourites", JSON.stringify([...this.favourites, photo]));
          this.favourites = [...this.favourites, photo];
        } else {
          localStorage.setItem("favourites", JSON.stringify(filtered));
          this.favourites = filtered;
        }
      }
    },
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

<style scoped>
.photos {
  display: flex;
  flex-direction: row;
}

.star {
  display: flex;
  font-size: 2em;
  margin-right: 10px;
}

.star.active {
  color: yellow;
}

.star:before {
  content: "\2605";
}

img {
  display: flex;
  width: 150px;
  height: 150px;
}

img:hover {
  cursor: pointer;
}
</style>