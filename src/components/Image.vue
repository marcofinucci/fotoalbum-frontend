<script>
export default {
  name: "Image",
  props: {
    photo: Object,
    backendUrl: String,
  },
};
</script>

<template>
  <div class="img-wrap rounded">
    <img
      class="img-fluid"
      :src="backendUrl + 'storage/' + photo.image"
      :alt="photo.title"
    />
  </div>
</template>

<style scoped>
.img-wrap {
  position: relative;
  overflow: hidden;
  aspect-ratio: 3 / 2;
  background-color: #1d1d1d;
}
.img-fluid {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
}
</style>
