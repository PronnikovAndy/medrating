<template>
  <div class="modal" v-if="previewUrl" @click.stop.prevent>
    <span class="close" @click.stop.prevent="$emit('hide-modal')">&times;</span>
    <img class="modal-content" :src="previewUrl">
  </div>
</template>

<script>
export default {
  props: ['previewUrl']
}
</script>

<style>
.modal {
  position: fixed;
  z-index: 1;
  padding-top: 100px;
  left: 0;
  top: 0;
  width: 100%;
  height: 100%;
  overflow: auto;
  background-color: rgb(0, 0, 0);
  background-color: rgba(0, 0, 0, 0.9);
}

.modal-content {
  margin: auto;
  display: block;
  width: 600px;
  height: 600px;
}

@keyframes zoom {
  from {
    transform: scale(0.1)
  }
  to {
    transform: scale(1)
  }
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

@media only screen and (max-width: 700px) {
  .modal-content {
    width: 100%;
  }
}
</style>

