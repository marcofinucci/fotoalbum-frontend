<script>
import Image from "./Image.vue";

export default {
  name: "Photos",
  components: {
    Image,
  },
  props: {
    photos: Array,
    baseUrl: String,
    currentPage: Number,
    lastPage: Number,
  },
  emits: ["loadMore"],
};
</script>

<template>
  <div class="row">
    <div class="col-lg-6 mt-4" v-for="photo in photos" :key="photo.id">
      <Image :baseUrl="baseUrl" :photo="photo" />
    </div>
  </div>
  <button
    type="button"
    v-if="currentPage < lastPage"
    class="btn btn-secondary mx-auto d-table mt-5"
    @click="$emit('loadMore')"
  >
    Mostra altro
  </button>
</template>
