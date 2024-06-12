<script>
import Image from "./Image.vue";

export default {
  name: "Photos",
  components: {
    Image,
  },
  props: {
    photos: Array,
    backendUrl: String,
    currentPage: Number,
    lastPage: Number,
  },
  emits: ["loadMore"],
  methods: {
    loadMore() {
      this.$emit("loadMore");
    },
  },
};
</script>

<template>
  <div class="row">
    <div class="col-6 mt-4" v-for="photo in photos" :key="photo.id">
      <Image :backendUrl="backendUrl" :photo="photo" />
    </div>
  </div>
  <button
    type="button"
    v-if="currentPage < lastPage"
    class="btn btn-primary mx-auto d-table mt-5"
    @click="loadMore()"
  >
    Mostra di più
  </button>
</template>
