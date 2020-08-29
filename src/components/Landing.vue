<template>
  <div class="landing">
    <!-- <div class="header relative">
      <div class="header-image"></div>
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
    </div>-->

    <div class="header">
      <div class="header-image"></div>
      <div class="w-100 h-100 fade-layer"></div>
      <div class="w-100 h-100 absolute t-0">
        <div class="text-left h-20">
          <img src="../assets/images/logo.svg" alt class="logo" />
        </div>
        <div class="text-left h-80 relative">
          <div>
            <div class="slogan-container">
              <h1 class="slogan">El secreto de tu cocina</h1>
            </div>
            <div class="hero-shape-container"></div>
          </div>
        </div>
      </div>
    </div>

    <div class="d-block mt-4">
      <div class="mb-3 w-100 relative">
        <div>
          <img src="../assets/images/heading-shape.png" alt class="heading-image" />
        </div>
        <h1 class="caveat heading-title">Nuestros artículos</h1>
      </div>
    </div>
    <div class="responsive-filter-box">
      <ul class="primary-color">
        <li class="responsive-filter mx-05">
          <a
            href="#/"
            @click="filterByCategory('Todos')"
            :class="[ currentCategory == 'Todos' ? ['active-color','bold-font'] : '']"
          >Todos</a>
        </li>
        <li class="responsive-filter mx-05">
          <a
            href="#/"
            @click="filterByCategory('Productos')"
            :class="[ currentCategory == 'Productos' ? ['active-color','bold-font'] : '']"
          >Productos</a>
        </li>
        <li class="responsive-filter mx-05">
          <a
            href="#/"
            @click="filterByCategory('Recetas')"
            :class="[ currentCategory == 'Recetas' ? ['active-color','bold-font'] : '']"
          >Recetas</a>
        </li>
        <li class="responsive-filter mx-05">
          <a
            href="#/"
            @click="filterByCategory('Consejos')"
            :class="[ currentCategory == 'Consejos' ? ['active-color','bold-font'] : '']"
          >Consejos</a>
        </li>
      </ul>
    </div>
    <div class="mid-section">
      <div class="filter-box soft-shadow mt-3 ml-1 primary-color br-10">
        <ul class="primary-color">
          <li class="mb-2">
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
            <item v-bind="item" :key="item.id" class="item-container"></item>
          </template>
          <item class="item-container"></item>
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
      this.loading = true;
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

.responsive-filter-box {
  width: 100%;
  height: 50px;
}

.filter-box ul,
.responsive-filter-box ul {
  font-family: "Open Sans";
  text-align: left;
  margin-left: 10px;
  list-style-type: none;
  padding: 0;
  text-transform: uppercase;
  width: 100%;
}

.filter-box ul a,
.responsive-filter-box ul a {
  text-decoration: none;
  font-size: 14px;
}

.filter-box ul li,
.responsive-filter-box ul li {
  position: relative;
}

.filter-box ul a,
.responsive-filter-box ul a {
  text-decoration: none;
  font-size: 14px;
}

.filter-box ul li,
.responsive-filter-box ul li {
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
  width: 50%;
  height: auto;
  margin-top: 30px;
}

.heading-title {
  margin-top: 0px;
  font-weight: bold;
  position: absolute;
  top: 20px;
  width: 100%;
}

.header {
  height: 400px;
  min-height: 200px;
  width: 100%;
  position: relative;
}

.header-image {
  height: 100%;
  position: relative;
  background: url("../assets/images/hero-background.jpg") no-repeat;
  background-size: cover;
  background-position: center;
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
  width: 90px;
  padding-top: 30px;
  padding-left: 30px;
}

.logo-image {
  background-image: url(../assets/images/logo.svg);
  background-repeat: no-repeat;
  width: 100%;
  height: 100%;
  top: 50px;
  left: 50px;
}

.hero-shape-container {
  background-image: url(../assets/images/hero-shape.png);
  position: absolute;
  top: 50%;
  left: -50%;
  width: 100%;
  height: 200px;
  background-size: contain;
  background-repeat: no-repeat;
}

.hero-shape {
  width: 70vw;
  margin-left: -40%;
}

.slogan {
  font-family: "Caveat";
  margin-top: 0px;
  margin-left: 30px;
}

.slogan-container {
  position: absolute;
  z-index: 999;
  width: 100%;
}

.slogan-heading-image {
  width: 100%;
}

.responsive-filter {
  float: left;
  display: block;
  font-size: 14px !important;
}

/* .heading-image-container {
  margin-left: -100%;
} */

@media screen and (max-width: 800px) {
  .heading-title {
    font-size: 40px;
  }

  .responsive-filter-box {
    display: block;
  }

  .filter-box {
    display: none;
  }

  .heading-image {
    width: 90%;
  }

  .item-container {
    margin: 0 auto;
    margin-top: 10px;
    margin-bottom: 10px;
  }

  .slogan {
    font-size: 40px;
    margin-top: 70px !important;
    width: 50% !important;
  }
}

@media screen and (min-width: 800px) {
  .heading-title {
    font-size: 5vw;
  }

  .responsive-filter-box {
    display: none;
  }

  .filter-box {
    display: flex;
  }

  .item-container {
    margin: 10px;
  }

  .slogan {
    margin-top: 30px;
    width: 30%;
    font-size: 6vw;
  }
}
</style>
