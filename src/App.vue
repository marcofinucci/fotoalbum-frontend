<script>
import axios from "axios";
import Alert from "./components/Alert.vue";
import Spinner from "./components/Spinner.vue";
import Photos from "./components/Photos.vue";
import Jumbotron from "./components/Jumbotron.vue";
import Footer from "./components/Footer.vue";

export default {
  name: "App",
  components: {
    Alert,
    Spinner,
    Photos,
    Jumbotron,
    Footer,
  },
  data() {
    return {
      photos: [],
      categories: [],
      baseUrl: "http://127.0.0.1:8000/",
      status: "loading",
      formData: {
        title: "",
        category: "All",
        featured: false,
      },
      lastApi: {
        currentPage: 0,
        lastPage: 0,
      },
    };
  },
  methods: {
    apiPhotos() {
      axios
        .get(this.baseUrl + "api/photos")
        .then((response) => {
          /* Save data */
          this.photos = response.data.results.data;

          /* Set pagination */
          this.lastApi.currentPage = response.data.results.current_page;
          this.lastApi.lastPage = response.data.results.last_page;
        })
        .catch((error) => {
          /* Error */
          this.status = "error";
          console.log(error);
        })
        .finally(() => {
          /* Set status */
          if (this.status != "error") {
            if (this.photos.length == 0) {
              this.status = "no-photos";
            } else {
              this.status = "loaded";
            }
          }
        });
    },

    apiCategories() {
      axios
        .get(this.baseUrl + "api/categories")
        .then((response) => {
          /* Save data */
          this.categories = response.data.results;
        })
        .catch((error) => {
          /* Error */
          console.log(error);
        });
    },

    apiSearch() {
      const search = "search=true";
      const title = "title=" + this.formData.title;
      const category = "category=" + this.formData.category;
      const featured = "featured=" + this.formData.featured;
      const url = this.baseUrl + "api/photos?" + search + "&" + title + "&" + category + "&" + featured;

      axios
        .get(url)
        .then((response) => {
          /* Save data */
          this.photos = response.data.results.data;

          /* Set pagination */
          this.lastApi.currentPage = response.data.results.current_page;
          this.lastApi.lastPage = response.data.results.last_page;
        })
        .catch((error) => {
          /* Error */
          this.status = "error";
          console.log(error);
        })
        .finally(() => {
          /* Set status */
          if (this.status != "error") {
            if (this.photos.length == 0) {
              this.status = "no-photos";
            } else {
              this.status = "loaded";
            }
          }
          console.log(url);
        });
    },

    resetForm() {
      this.formData.title = "";
      this.formData.category = "All";
      this.formData.featured = false;
      this.apiSearch();
    },

    loadMore() {},
  },
  mounted() {
    this.apiPhotos();
    this.apiCategories();
  },
};
</script>

<template>
  <!-- Header -->
  <header id="app-header">
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
          <form @submit.prevent="apiSearch()" class="row w-100 flex-nowrap align-items-center" role="search">
            <!-- Title -->
            <div class="col me-2">
              <input
                class="form-control"
                type="search"
                placeholder="Cerca per titolo"
                aria-label="Search"
                v-model="formData.title"
              />
            </div>
            <!-- Category -->
            <div class="col-auto me-2">
              <select class="form-select" aria-label="Default select example" v-model="formData.category">
                <option value="All" selected>Tutte le categorie</option>
                <option v-for="category in categories" :key="category.id" :value="category.id">
                  {{ category.name }}
                </option>
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
                  v-model="formData.featured"
                />
                <label class="form-check-label" for="flexSwitchCheckChecked">In evidenza</label>
              </div>
            </div>
            <!-- Submit -->
            <div class="col-auto">
              <button class="btn btn-primary me-2" type="submit">
                <i class="bi bi-search me-2"></i>
                <span>Cerca</span>
              </button>
              <button class="btn btn-link" type="button" @click="resetForm()">
                <span>Resetta i campi</span>
              </button>
            </div>
          </form>
        </div>
      </div>
    </nav>
  </header>

  <!-- Main -->
  <main id="app-main">
    <!-- Jumbotron -->
    <Jumbotron />

    <div class="container mb-5 pb-5">
      <!-- Loading -->
      <div class="mt-5" v-if="this.status == 'loading'">
        <Spinner />
      </div>

      <!-- Error -->
      <div class="mt-5" v-else-if="this.status == 'error'">
        <Alert title="Qualcosa è andato storto" />
      </div>

      <!-- No photos -->
      <div class="mt-5" v-else-if="this.status == 'no-photos'">
        <Alert title="Nessuna foto trovata" />
      </div>

      <!-- Loaded -->
      <div v-else-if="this.status == 'loaded'">
        <Photos
          :photos="photos"
          :baseUrl="baseUrl"
          :currentPage="this.lastApi.currentPage"
          :lastPage="this.lastApi.lastPage"
          @loadMore="loadMore()"
        />
      </div>
    </div>
  </main>

  <!-- Footer -->
  <footer id="app-footer">
    <Footer />
  </footer>
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
