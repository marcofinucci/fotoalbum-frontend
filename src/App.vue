<script>
import axios from "axios";
import Alert from "./components/Alert.vue";
import Spinner from "./components/Spinner.vue";
import Photos from "./components/Photos.vue";
import Navbar from "./components/Navbar.vue";
import Jumbotron from "./components/Jumbotron.vue";
import Footer from "./components/Footer.vue";

export default {
  name: "App",
  components: {
    Alert,
    Spinner,
    Photos,
    Navbar,
    Jumbotron,
    Footer,
  },
  data() {
    return {
      photos: [],
      currentPage: 0,
      lastPage: 0,
      backendUrl: "http://127.0.0.1:8000/",
      status: "loading",
    };
  },
  mounted() {
    this.apiCall(1);
  },
  methods: {
    apiCall(page) {
      axios
        .get(this.backendUrl + "api/photos?page=" + page)
        .then((response) => {
          this.photos = [...this.photos, ...response.data.results.data];
          this.currentPage = response.data.results.current_page;
          this.lastPage = response.data.results.last_page;
        })
        .catch((error) => {
          this.status = "error";
          console.log(error);
        })
        .finally(() => {
          if (this.status != "error") {
            if (this.photos.length == 0) {
              this.status = "no-photos";
            } else {
              this.status = "loaded";
            }
          }
        });
    },
    loadMore() {
      this.apiCall(this.currentPage + 1);
      this.currentPage = this.currentPage + 1;
    },
  },
};
</script>

<template>
  <!-- Header -->
  <header id="app-header">
    <Navbar />
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
          :backendUrl="backendUrl"
          :currentPage="currentPage"
          :lastPage="lastPage"
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
