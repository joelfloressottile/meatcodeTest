<template>
  <div class="newsletter">
    <div class="mb-3 w-100 relative">
      <div>
        <img src="../assets/images/heading-shape.png" alt class="heading-image" />
      </div>
      <h1 class="caveat heading-title">Contáctanos</h1>
    </div>

    <form @submit.prevent="checkContactForm" method="post">
      <div class="form-container">
        <div class="mb-2">
          <div class="form-item d-block">
            <label class="form-label text-left d-block mb-05">Nombre</label>
            <input
              v-model.trim="$v.contactForm.firstName.$model"
              :class="{'input-has-error': $v.contactForm.firstName.$invalid && $v.contactForm.firstName.$dirty}"
              type="text"
              class="form-input d-block w-80"
            />
          </div>
        </div>

        <div class="mb-2">
          <div class="form-item d-block">
            <label class="form-label text-left d-block mb-05">Apellido</label>
            <input
              v-model.trim="$v.contactForm.lastName.$model"
              :class="{'input-has-error': $v.contactForm.lastName.$invalid && $v.contactForm.lastName.$dirty}"
              type="text"
              class="form-input d-block w-80"
            />
          </div>
        </div>

        <div class="mb-2">
          <div class="form-item d-block">
            <label class="form-label text-left d-block mb-05">Mail</label>
            <input
              v-model.trim="$v.contactForm.email.$model"
              :class="{'input-has-error': $v.contactForm.email.$invalid && $v.contactForm.email.$dirty}"
              type="text"
              class="form-input d-block w-80"
            />
          </div>
        </div>

        <div class="mb-2">
          <div class="form-item d-block">
            <label class="form-label text-left d-block mb-05">Teléfono</label>
            <input
              v-model.trim="$v.contactForm.phone.$model"
              :class="{'input-has-error': $v.contactForm.phone.$invalid && $v.contactForm.phone.$dirty}"
              type="text"
              class="form-input d-block w-80"
            />
          </div>
        </div>

        <div class="d-block btn-item text-right">
          <label v-if="subscribed" class="open-sans mr-1">¡Suscrito con éxito!</label>
          <label
            v-if="!subscribed && error"
            class="open-sans mr-1"
          >Tuvimos un problema :( Vuelve a intentar</label>
          <button type="submit" class="btn" :disabled="this.$v.$invalid">Enviar</button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
import {
  required,
  email,
  minLength,
  maxLength,
  numeric
} from "vuelidate/lib/validators";
import axios from "axios";

export default {
  name: "NewsletterForm",
  data() {
    return {
      contactForm: {
        firstName: "",
        lastName: "",
        email: "",
        phone: ""
      },
      subscribed: false,
      error: false
    };
  },
  validations: {
    contactForm: {
      firstName: {
        required
      },
      lastName: {
        required
      },
      email: {
        required,
        email
      },
      phone: {
        required,
        numeric,
        minLength: minLength(8),
        maxLength: maxLength(8)
      }
    }
  },
  methods: {
    checkContactForm() {
      this.$v.$touch();

      if (!this.$v.$invalid) {
        console.log("Valid form");
        this.subscribeContact();
      } else {
        console.log("Invalid form");
      }
    },
    subscribeContact() {
      axios
        .post("https://5eed24da4cbc340016330f0d.mockapi.io/api/subscribe", {
          firstname: this.contactForm.firstName,
          lastname: this.contactForm.lastName,
          email: this.contactForm.email,
          phone: this.contactForm.phone
        })
        .then(response => {
          console.log("Response from server: ", response);
          this.error = false;
          this.subscribed = true;
        })
        .catch(error => {
          console.log("Error from server: ", error);
          this.subscribed = false;
          this.error = true;
        });
    }
  }
};
</script>

<style scoped>
.form-container {
  width: 50%;
  margin: 0 auto;
  display: flex;
  flex-wrap: wrap;
}

.form-container > div {
  flex: 1 50%;
}

.form-item {
  width: 100%;
}

.btn-item {
  width: 100%;
}

.heading-image {
  filter: opacity(0.3);
}

.heading-title {
  margin-top: 0px;
  font-weight: bold;
  position: absolute;
  top: 20px;
  width: 100%;
  font-size: 50px;
}
</style>
