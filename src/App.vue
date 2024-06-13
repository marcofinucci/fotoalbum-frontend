<script>
import axios from "axios";
import Alert from "./components/Alert.vue";
import Spinner from "./components/Spinner.vue";
import Photos from "./components/Photos.vue";
import Jumbotron from "./components/Jumbotron.vue";
import Footer from "./components/Footer.vue";
import Header from "./components/Header.vue";

export default {
  name: "App",
  components: {
    Alert,
    Spinner,
    Photos,
    Jumbotron,
    Footer,
    Header,
  },
  data() {
    return {
      photos: [],
      categories: [],
      baseUrl: "http://127.0.0.1:8000/",
      status: "loading",
      formData: {
        title: "",
        category: "all",
        featured: "false",
      },
      lastApi: {
        currentPage: 0,
        lastPage: 0,
        isQuery: false,
        query: "",
      },
    };
  },
  methods: {
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
    apiPhotos(url) {
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
        });
    },
    apiSearch() {
      const search = "search=true";
      const title = "title=" + this.formData.title;
      const category = "category=" + this.formData.category;
      const featured = "featured=" + this.formData.featured;

      this.lastApi.isQuery = true;
      this.lastApi.query = search + "&" + title + "&" + category + "&" + featured;

      const url = this.baseUrl + "api/photos?" + this.lastApi.query;
      this.apiPhotos(url);

      console.log(url);
    },
    apiLoadMore() {
      let url = this.baseUrl + "api/photos?page=" + (parseFloat(this.lastApi.currentPage) + 1);

      if (this.lastApi.isQuery) {
        url += "&" + this.lastApi.query;
      }

      axios
        .get(url)
        .then((response) => {
          /* Save data */
          this.photos = [...this.photos, ...response.data.results.data];

          /* Set pagination */
          this.lastApi.currentPage = response.data.results.current_page;
          this.lastApi.lastPage = response.data.results.last_page;
        })
        .catch((error) => {
          /* Error */
          console.log(error);
        });
    },
  },
  mounted() {
    const url = this.baseUrl + "api/photos";
    this.apiPhotos(url);
    this.apiCategories();
  },
};
</script>

<template>
  <!-- Header -->
  <header id="app-header">
    <Header
      v-model:title="formData.title"
      v-model:category="formData.category"
      v-model:featured="formData.featured"
      :categories="categories"
      @search="apiSearch()"
    />
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
          @loadMore="apiLoadMore()"
        />
      </div>
    </div>
  </main>

  <!-- Footer -->
  <footer id="app-footer">
    <Footer />
  </footer>
</template>
