<script>
import axios from "axios";

export default {
  name: "Contact",
  props: {
    baseUrl: String,
  },
  data() {
    return {
      name: "",
      email: "",
      message: "",
      status: "",
      validationsErrors: null,
      error: false,
      loading: false,
    };
  },
  methods: {
    submitForm() {
      /* Reset errors and status */
      this.loading = true;
      this.status = "";
      this.validationsErrors = null;

      // Create payload
      const payload = {
        name: this.name,
        email: this.email,
        message: this.message,
      };

      // Post request
      axios
        .post(this.baseUrl + "api/contacts", payload)
        .then((response) => {
          this.loading = false;

          /* Check response */
          if (response.data.success) {
            this.status = "success";
            this.name = "";
            this.email = "";
            this.message = "";
          } else {
            this.status = "error";
            this.validationsErrors = response.data.error;
          }
        })
        .catch((error) => {
          /* Error */
          console.log(error);
          this.error = "error";
        });
    },
  },
};
</script>

<template>
  <!-- Contact button -->
  <button
    class="btn btn-primary btn-lg rounded-circle position-fixed bottom-0 end-0 me-3 mb-3"
    type="button"
    aria-label="Contattami"
    data-bs-toggle="modal"
    data-bs-target="#modal-contact"
  >
    <i class="bi bi-chat-dots"></i>
  </button>

  <!-- Modal -->
  <div class="modal fade" id="modal-contact" tabindex="-1" aria-labelledby="modal-contact-label" aria-hidden="true">
    <div class="modal-dialog modal-dialog-centered modal-lg modal-dialog-scrollable">
      <div class="modal-content">
        <div class="modal-header">
          <h1 class="modal-title fs-3" id="modal-contact-label">Contattami</h1>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body mb-2">
          <!-- Alert success -->
          <div v-if="status == 'success'" class="alert alert-success fade show mb-4" role="alert">
            <span>Messaggio inviato correttamente</span>
          </div>

          <!-- Alert error -->
          <div v-else-if="error" class="alert alert-danger fade show mb-4" role="alert">
            <span>Ops, qualcosa è andato storto</span>
          </div>

          <!-- Form -->
          <form @submit.prevent="submitForm()">
            <!-- Name -->
            <div class="mb-4">
              <label for="name" class="form-label">Nome</label>
              <input
                type="text"
                name="name"
                id="name"
                :class="'form-control ' + (validationsErrors && validationsErrors.name ? ' is-invalid' : '')"
                v-model="name"
              />
              <div class="form-text text-danger" v-if="validationsErrors && validationsErrors.name">
                {{ validationsErrors.name[0] }}
              </div>
            </div>

            <!-- Email -->
            <div class="mb-4">
              <label for="email" class="form-label">Email</label>
              <input
                type="text"
                name="email"
                id="email"
                :class="'form-control ' + (validationsErrors && validationsErrors.email ? ' is-invalid' : '')"
                v-model="email"
              />
              <div class="form-text text-danger" v-if="validationsErrors && validationsErrors.email">
                {{ validationsErrors.email[0] }}
              </div>
            </div>

            <!-- Message -->
            <div class="mb-4">
              <label for="message" class="form-label">Messaggio</label>
              <textarea
                name="message"
                id="message"
                :class="'form-control ' + (validationsErrors && validationsErrors.message ? ' is-invalid' : '')"
                rows="5"
                v-model="message"
              ></textarea>
              <div class="form-text text-danger" v-if="validationsErrors && validationsErrors.message">
                {{ validationsErrors.message[0] }}
              </div>
            </div>

            <!-- Submit -->
            <button class="btn btn-primary" type="submit" :disabled="loading">
              <span v-if="loading" class="me-2">
                <span class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>
                <span class="visually-hidden">Caricamento...</span>
              </span>
              <i class="bi bi-send me-2" v-else></i>
              <span>Invia</span>
            </button>
          </form>
        </div>
      </div>
    </div>
  </div>
</template>
