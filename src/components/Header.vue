<script>
export default {
  name: "Header",
  props: {
    title: String,
    category: String,
    featured: String,
    categories: Array,
  },
  emits: ["update:title", "update:category", "update:featured", "search"],
};
</script>

<template>
  <nav class="navbar navbar-expand-lg fixed-top border-bottom border-body py-3">
    <div class="container-fluid">
      <!-- Logo -->
      <div class="navbar-brand text-secondary-emphasis pe-5">FOTO ALBUM</div>
      <!-- Toggle -->
      <button
        class="navbar-toggler"
        type="button"
        data-bs-toggle="collapse"
        data-bs-target="#navbarToggler"
        aria-controls="navbarToggler"
        aria-expanded="false"
        aria-label="Toggle navigation"
      >
        <span class="navbar-toggler-icon"></span>
      </button>

      <!-- Collapse -->
      <div class="collapse navbar-collapse" id="navbarToggler">
        <!-- Search -->
        <form @submit.prevent="$emit('search')" class="row w-100 align-items-center mt-4 mt-lg-0" role="search">
          <!-- Title -->
          <div class="col-12 col-lg me-2">
            <input
              class="form-control"
              type="search"
              placeholder="Cerca per titolo"
              aria-label="Search"
              :value="title"
              @input="$emit('update:title', $event.target.value)"
            />
          </div>
          <!-- Category -->
          <div class="col-12 col-lg-auto me-lg-2 mt-3 mt-lg-0">
            <select
              class="form-select"
              aria-label="Default select example"
              :value="category"
              @input="$emit('update:category', $event.target.value)"
            >
              <option value="all" selected>Tutte le categorie</option>
              <option v-for="category in categories" :key="category.id" :value="category.id">
                {{ category.name }}
              </option>
            </select>
          </div>
          <!-- Featured -->
          <div class="col-12 col-lg-auto me-lg-2 mt-3 mt-lg-0">
            <div class="form-check form-switch mb-0">
              <input
                class="form-check-input"
                type="checkbox"
                role="switch"
                id="featured"
                :value="featured == 'true' ? 'false' : 'true'"
                @input="$emit('update:featured', $event.target.value)"
              />
              <label class="form-check-label" for="featured">
                <i class="bi bi-star-fill me-2"></i>
                <span>In evidenza</span>
              </label>
            </div>
          </div>
          <!-- Submit -->
          <div class="col-12 col-lg-auto mt-3 mt-lg-0">
            <button class="btn btn-primary" type="submit">
              <i class="bi bi-search me-2"></i>
              <span>Cerca</span>
            </button>
          </div>
        </form>
      </div>
    </div>
  </nav>
</template>

<style scoped>
.navbar {
  backdrop-filter: blur(10px);
  background-color: rgb(0 0 0 / 80%);
}
.navbar-brand {
  font-weight: 600;
  font-size: 1.125rem;
  letter-spacing: 0.0625rem;
}
</style>
