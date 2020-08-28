<template>
  <div class="item-container soft-shadow br-10 no-overflow relative primary-color">
    <div v-if="id!=-1">
      <img :src="image" alt class="image" />
    </div>
    <div v-if="id!=-1" class="open-sans pa-2">
      <h4 class="text-header text-left">{{title}}</h4>
      <p class="text-content text-left">{{content}}</p>
      <p class="mb-1 text-left">
        <a href class="extra-text">Ver más</a>
      </p>
    </div>
    <div v-else class="vertical-center w-inherit open-sans">
      <span class="search-icon">
        <i class="fa fa-search"></i>
      </span>
      <p>
        <a href="#/" class="last-card-text">Ver todo</a>
      </p>
    </div>
  </div>
</template>

<script>
export default {
  name: "Item",
  props: {
    id: {
      type: Number,
      default: -1
    },
    createdAt: {
      type: String,
      default: "Someday"
    },
    title: {
      type: String,
      default: ""
    },
    category: {
      type: String,
      default: "Any"
    },
    image: {
      type: String,
      default: ""
    },
    content: {
      type: String,
      default: "Ver todo el inventario"
    },
    url: {
      type: String,
      default: "asd"
    }
  },
  data() {
    return {
      msg: "Here goes the list"
    };
  }
};
</script>

<style scoped>
.item-container {
  width: 250px !important;
  min-width: 250px !important;
}

.item-container:hover .extra-text {
  display: block;
}

.text-header {
  font-size: 16px;
}

.text-content {
  font-size: 14px;
}
/* 
#hover-content {
  display: none;
}
#parent:hover #hover-content {
  display: block;
} */

.extra-text {
  color: #d8ad3d !important;
  text-transform: uppercase;
  font-weight: bold;
  text-align: left;
  text-decoration: none;
  display: none;
  font-size: 14px;
}

.last-card-text {
  color: grey;
  text-transform: uppercase;
  font-weight: bold;
  text-align: center;
  text-decoration: none;
  font-size: 14px;
}

.image {
  max-width: 100%;
}

.search-icon {
  font-size: 40px;
}
</style>
