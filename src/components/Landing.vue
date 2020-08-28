<template>
  <div class="landing">
    <newsletter></newsletter>
  </div>
</template>

<script>
import NewsletterForm from "@/components/NewsletterForm.vue";
export default {
  name: "Landing",
  components: {
    Newsletter: NewsletterForm
  },
  data() {
    return {
      msg: "Whatever"
    };
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
