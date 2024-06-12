<script>
export default {
  name: "Image",
  props: {
    photo: Object,
    baseUrl: String,
  },
};
</script>

<template>
  <div class="img-wrap rounded">
    <!-- If photo.image start with http -->
    <img v-if="photo.image.startsWith('http')" class="img-fluid" :src="photo.image" :alt="photo.title" loading="lazy" />
    <!-- else -->
    <img v-else class="img-fluid" :src="baseUrl + 'storage/' + photo.image" :alt="photo.title" loading="lazy" />
  </div>
  <div>{{ photo.title }}</div>
  <div>{{ photo.featured }}</div>
  <div>{{ photo.category?.id }}</div>
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
