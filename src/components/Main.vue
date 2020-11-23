<template>
  <main>
    <ul>
      <li
          @click.prevent="getAlbums(user.id)"
          v-for="user of users"
          :key="user.id"
      >
        <span
            :class="[!users.length ? 'arrow-down' : 'arrow-right']"
        >{{ user.name }}</span>
        <ul
            class="embedded"
            v-if="albums.length && albums[0].userId === user.id"
        >
          <li
              @click.stop.prevent="getPhotos(album.id)"
              v-for="album of albums"
              :key="album.id"
          >
            <span
                :class="[!albums.length ? 'arrow-down' : 'arrow-right']"
            >{{ album.title }}</span>
            <ul
                v-if="photos.length && photos[0].albumId === album.id"
            >
              <li
                  @click.stop.prevent
                  v-for="photo of photos"
                  :key="photo.id"
                  class="photos"
              >
                <span
                    :class="favourites && favourites.some(t => t.id === photo.id) ? 'star active' : 'star'"
                    @click="addToFavourites(photo)"
                ></span>
                <img
                    @click="showImageModal(photo.url)"
                    :alt="photo.title"
                    :src="photo.thumbnailUrl"
                />
              </li>
            </ul>
          </li>
        </ul>
      </li>
    </ul>
    <div class="modal" v-if="previewUrl">
      <span class="close" @click="hideImageModal">&times;</span>
      <img class="modal-content" :src="previewUrl">
      <div id="caption"></div>
    </div>
  </main>
</template>

<script>
export default {
  name: 'Content',
  data() {
    return {
      users: [],
      albums: [],
      photos: [],
      previewUrl: null,
      favourites: JSON.parse(localStorage.getItem("favourites"))
    }
  },
  mounted() {
    fetch('https://json.medrating.org/users/')
        .then(response => response.json())
        .then(data => data.filter(t => t.name))
        .then(data => this.users = data);
  },
  methods: {
    getAlbums(userId) {
      if (!this.albums.length) {
        fetch(`https://json.medrating.org/albums?userId=${userId}`)
            .then(response => response.json())
            .then(data => this.albums = data);
      } else {
        this.albums = [];
      }
    },
    getPhotos(albumId) {
      if (!this.photos.length) {
        fetch(`https://json.medrating.org/photos?albumId=${albumId}`)
            .then(response => response.json())
            .then(data => this.photos = data);
      } else {
        this.photos = [];
      }
    },
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
  }
}
</script>

<style>
ul {
  list-style: none;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

li {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  padding: 10px;
  margin: 10px 0;
}

.embedded {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  padding-left: 30px;
}

img:hover, span:hover {
  cursor: pointer;
}

img {
  display: flex;
  width: 150px;
  height: 150px;
}

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

.arrow-right:before {
  content: '\003E';
  margin-right: 10px;
}

.arrow-down:before {
  content: '\02C5';
  margin-right: 10px;
}

.modal {
  position: fixed;
  z-index: 1;
  padding-top: 100px;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgb(0,0,0);
  background-color: rgba(0,0,0,0.9);
}

.modal-content {
  margin: auto;
  display: block;
  width: 600px;
  height: 600px;
}

@keyframes zoom {
  from {transform: scale(0.1)}
  to {transform: scale(1)}
}

.close {
  position: absolute;
  top: 15px;
  right: 35px;
  color: #f1f1f1;
  font-size: 40px;
  font-weight: bold;
  transition: 0.3s;
}

.close:hover,
.close:focus {
  color: #bbb;
  text-decoration: none;
  cursor: pointer;
}

@media only screen and (max-width: 700px){
  .modal-content {
    width: 100%;
  }
}
</style>