<template>
  <div class="landing">
    <div class="mid-section">
      <div class="filter-box soft-shadow mt-1 ml-1 primary-color br-10">
        <ul class="primary-color">
          <li class="my-2">
            <a
              href="#"
              @click="filterByCategory('Todos')"
              :class="[ currentCategory == 'Todos' ? ['active-color','bold-font'] : '']"
            >Todos</a>
            <span v-if="currentCategory == 'Todos'" class="absolute r-2 active-color">></span>
          </li>
          <li class="mb-2">
            <a
              href="#"
              @click="filterByCategory('Productos')"
              :class="[ currentCategory == 'Productos' ? ['active-color','bold-font'] : '']"
            >Productos</a>
            <span v-if="currentCategory == 'Productos'" class="absolute r-2 active-color">></span>
          </li>
          <li class="mb-2">
            <a
              href="#"
              @click="filterByCategory('Recetas')"
              :class="[ currentCategory == 'Recetas' ? ['active-color','bold-font'] : '']"
            >Recetas</a>
            <span v-if="currentCategory == 'Recetas'" class="absolute r-2 active-color">></span>
          </li>
          <li class="mb-2">
            <a
              href="#"
              @click="filterByCategory('Consejos')"
              :class="[ currentCategory == 'Consejos' ? ['active-color','bold-font'] : '']"
            >Consejos</a>
            <span v-if="currentCategory == 'Consejos'" class="absolute r-2 active-color">></span>
          </li>
        </ul>
      </div>
      <div class="right-box list-of-items mb-4">
        <template v-for="item in itemsToShow">
          <item v-bind="item" :key="item.id" class="mx-1 my-1"></item>
        </template>
        <item class="mx-1 my-1"></item>
      </div>
    </div>

    <newsletter></newsletter>
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
      currentCategory: "Todos"
    };
  },
  methods: {
    getAllItems() {
      axios
        .get("https://5eed24da4cbc340016330f0d.mockapi.io/api/articles")
        .then(response => {
          console.log("Response from request: ", response);
          this.items = response.data;
          this.itemsToShow = this.items.slice(0, 5);
          console.log("Items: ", this.items);
        })
        .catch(error => {
          console.log("Error from request: ", error);
        });
    },
    filterByCategory(category) {
      if (this.currentCategory === category) return;
      else {
        this.currentCategory = category;
        if (category === "Todos") this.itemsToShow = this.items.slice(0, 5);
        else {
          this.itemsToShow = this.items.filter(c => c.category == category);
          if (this.itemsToShow.length > 5)
            this.itemsToShow = this.itemsToShow.slice(0, 5);
        }
      }
    }
  },
  // Life cycle events
  mounted() {
    this.getAllItems();
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
</style>
