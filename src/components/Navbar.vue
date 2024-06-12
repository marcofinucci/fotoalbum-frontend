<script>
export default {
  name: "Navbar",
};
</script>

<template>
  <nav
    class="navbar bg-dark navbar-expand-lg fixed-top border-bottom border-body py-3"
  >
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
        <form class="row w-100 flex-nowrap align-items-center" role="search">
          <!-- Title -->
          <div class="col me-2">
            <input
              class="form-control"
              type="search"
              placeholder="Cerca per titolo"
              aria-label="Search"
            />
          </div>
          <!-- Category -->
          <div class="col-auto me-2">
            <select class="form-select" aria-label="Default select example">
              <option selected>Open this select menu</option>
              <option value="1">One</option>
              <option value="2">Two</option>
              <option value="3">Three</option>
            </select>
          </div>
          <!-- Featured -->
          <div class="col-auto me-2">
            <div class="form-check form-switch mb-0">
              <input
                class="form-check-input"
                type="checkbox"
                role="switch"
                id="flexSwitchCheckChecked"
                checked
              />
              <label class="form-check-label" for="flexSwitchCheckChecked"
                >In evidenza</label
              >
            </div>
          </div>
          <!-- Submit -->
          <div class="col-auto">
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
.navbar-brand {
  font-weight: 600;
  font-size: 1.125rem;
  letter-spacing: 0.0625rem;
}
</style>
