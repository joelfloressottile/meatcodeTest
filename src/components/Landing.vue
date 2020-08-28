<template>
  <div class="landing">
    <div class="header relative">
      <div class="logo">
        <div class="logo-image"></div>
      </div>
      <div class="slogan">
        <div class="relative">
          <div class="heading-image-container">
            <img src="../assets/images/hero-shape.png" alt class="slogan-heading-image" />
          </div>
          <p class="caveat heading-title slogan-text">El secreto de tu cocina</p>
        </div>
      </div>
      <div class="w-100 h-100 fade-layer"></div>
    </div>

    <div class="d-block mt-4">
      <div class="mb-3 w-100 relative">
        <div>
          <img src="../assets/images/heading-shape.png" alt class="heading-image" />
        </div>
        <h1 class="caveat heading-title">Nuestros artículos</h1>
      </div>
    </div>
    <div class="mid-section">
      <div class="filter-box soft-shadow mt-3 ml-1 primary-color br-10">
        <ul class="primary-color">
          <li class="my-2">
            <a
              href="#/"
              @click="filterByCategory('Todos')"
              :class="[ currentCategory == 'Todos' ? ['active-color','bold-font'] : '']"
            >Todos</a>
            <span v-if="currentCategory == 'Todos'" class="arrow-icon">
              <i class="fa fa-arrow-right"></i>
            </span>
          </li>
          <li class="mb-2">
            <a
              href="#/"
              @click="filterByCategory('Productos')"
              :class="[ currentCategory == 'Productos' ? ['active-color','bold-font'] : '']"
            >Productos</a>
            <span v-if="currentCategory == 'Productos'" class="arrow-icon">
              <i class="fa fa-arrow-right"></i>
            </span>
          </li>
          <li class="mb-2">
            <a
              href="#/"
              @click="filterByCategory('Recetas')"
              :class="[ currentCategory == 'Recetas' ? ['active-color','bold-font'] : '']"
            >Recetas</a>
            <span v-if="currentCategory == 'Recetas'" class="arrow-icon">
              <i class="fa fa-arrow-right"></i>
            </span>
          </li>
          <li class="mb-2">
            <a
              href="#/"
              @click="filterByCategory('Consejos')"
              :class="[ currentCategory == 'Consejos' ? ['active-color','bold-font'] : '']"
            >Consejos</a>
            <span v-if="currentCategory == 'Consejos'" class="arrow-icon">
              <i class="fa fa-arrow-right"></i>
            </span>
          </li>
        </ul>
      </div>
      <transition name="fade">
        <div v-if="!loading" class="right-box mt-2 list-of-items mb-4">
          <template v-for="item in itemsToShow">
            <item v-bind="item" :key="item.id" class="mx-1 my-1"></item>
          </template>
          <item class="mx-1 my-1"></item>
        </div>
      </transition>
    </div>

    <newsletter class="pa-4"></newsletter>
  </div>
</template>

<script>
import NewsletterForm from "@/components/NewsletterForm.vue";
import Item from "@/components/item.vue";
import axios from "axios";

export default {
  name: "Landing",
  components: {
    newsletter: NewsletterForm,
    item: Item
  },
  data() {
    return {
      items: [],
      itemsToShow: [],
      currentCategory: "Todos",
      loading: true
    };
  },
  methods: {
    getItemsByCategory(category) {
      let filter = category === "Todos" ? "" : "?filter=" + category;
      console.log(
        "Url to request: ",
        "https://5eed24da4cbc340016330f0d.mockapi.io/api/articles" + filter
      );

      axios
        .get(
          "https://5eed24da4cbc340016330f0d.mockapi.io/api/articles" + filter
        )
        .then(response => {
          console.log("Response from request: ", response);
          this.items = response.data;
          this.itemsToShow = this.items.slice(0, 5);
          console.log("Items: ", this.items);
        })
        .catch(error => {
          console.log("Error from request: ", error);
        })
        .finally(() => (this.loading = false));
    },
    filterByCategory(category) {
      if (this.currentCategory === category) {
        console.log("Same category, do nothing");
      } else {
        this.currentCategory = category;
        this.getItemsByCategory(this.currentCategory);
      }
    }
  },
  // Life cycle events
  mounted() {
    this.getItemsByCategory("Todos");
  }
};
</script>

<style scoped>
.list-of-items {
  display: flex;
  flex-flow: row wrap;
}

.mid-section {
  display: flex;
  overflow: auto;
}

.filter-box {
  height: 200px;
  min-width: 200px;
  width: 200px;
}

.filter-box ul {
  font-family: "Open Sans";
  text-align: left;
  margin-left: 10px;
  list-style-type: none;
  padding: 0;
  text-transform: uppercase;
}

.filter-box ul a {
  text-decoration: none;
  font-size: 12px;
}

.filter-box ul li {
  position: relative;
}

.landing {
  background: #fafafa;
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 2s;
}
.fade-enter,
.fade-leave-to {
  opacity: 0;
  transition: opacity 2s;
}

.arrow-icon {
  position: absolute;
  right: 20px;
  top: 3px;
  font-size: 14px;
}

.fa {
  color: #d8ad3d !important;
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

.header {
  height: 50vh;
  width: 100%;
  background-image: url(../assets/images/hero-background.jpg);
  background-size: cover;
}

.fade-layer {
  background: #fafafa;
  background: linear-gradient(
    0deg,
    #fafafa 0%,
    #fafafa 0%,
    rgba(0, 212, 255, 0) 35%
  );
  position: absolute;
  top: 0;
}

.logo {
  width: 25%;
  height: 25%;
  padding: 50px 0px 0px 50px;
}

.logo-image {
  background-image: url(../assets/images/logo.svg);
  background-repeat: no-repeat;
  width: 100%;
  height: 100%;
  top: 50px;
  left: 50px;
}

.slogan {
  height: 75%;
  width: 50%;
}

.slogan-text {
  text-align: left;
  padding-left: 50px;
  font-size: 82px;
}

.slogan-heading-image {
  width: 100%;
}

.heading-image-container {
  margin-left: -100%;
}
</style>
