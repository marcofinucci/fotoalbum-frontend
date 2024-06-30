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
  <!-- Image -->
  <div data-bs-toggle="modal" :data-bs-target="'#modal-' + photo.id" class="img-wrap rounded">
    <!-- If photo.image start with http -->
    <img v-if="photo.image.startsWith('http')" class="img-fluid" :src="photo.image" :alt="photo.title" loading="lazy" />
    <!-- else -->
    <img v-else class="img-fluid" :src="baseUrl + 'storage/' + photo.image" :alt="photo.title" loading="lazy" />
  </div>
  <!-- Info -->
  <div class="d-flex align-items-center flex-wrap justify-content-between">
    <div class="me-3 mt-2">{{ photo.title }}</div>
    <div class="d-flex align-items-center">
      <i v-if="photo.featured" class="bi bi-star-fill me-2 mt-2"></i>
      <div v-if="photo.category" class="badge bg-secondary rounded-pill fw-medium mt-2">{{ photo.category?.name }}</div>
    </div>
  </div>

  <!-- Modal -->
  <div
    class="modal fade"
    :id="'modal-' + photo.id"
    tabindex="-1"
    :aria-labelledby="'modal-label-' + photo.id"
    aria-hidden="true"
  >
    <div class="modal-dialog modal-dialog-centered modal-xl modal-dialog-scrollable">
      <div class="modal-content">
        <div class="modal-header">
          <h1 class="modal-title fs-3" :id="'modal-label-' + photo.id">{{ photo.title }}</h1>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body mb-2">
          <!-- Featured -->
          <div v-if="photo.featured" class="text-primary mb-1">
            <i class="bi bi-star-fill"></i> <span>In evidenza</span>
          </div>
          <!-- Category -->
          <div class="mb-2">
            <span class="text-secondary-emphasis">Categoria: </span>
            <span v-if="photo.category">{{ photo.category.name }}</span>
            <span v-else>Nessuna</span>
          </div>
          <!-- Description -->
          <div v-if="photo.description" class="fs-4 mb-4">{{ photo.description }}</div>
          <!-- Image -->
          <div>
            <!-- If photo.image start with http -->
            <img
              v-if="photo.image.startsWith('http')"
              class="img-fluid rounded"
              :src="photo.image"
              :alt="photo.title"
              loading="lazy"
            />
            <!-- else -->
            <img
              v-else
              class="img-fluid rounded"
              :src="baseUrl + 'storage/' + photo.image"
              :alt="photo.title"
              loading="lazy"
            />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.img-wrap {
  position: relative;
  overflow: hidden;
  aspect-ratio: 3 / 2;
  background-color: #1d1d1d;
  cursor: pointer;
}

.img-wrap img {
  transition: 1s;
}

.img-wrap:hover img {
  transform: scale(1.1);
}

.img-fluid {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
}
</style>
