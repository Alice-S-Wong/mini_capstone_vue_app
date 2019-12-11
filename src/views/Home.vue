<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>{{ firstProduct }}</p>
    <p>{{ secondProduct }}</p>
    <p>{{ thirdProduct }}</p>
  </div>
</template>

<style>
</style>

<script>
export default {
  data: function() {
    return {
      message: "Welcome to Vue.js!",
      firstProduct: "Bag of Bones",
      secondProduct: "Humorous Humerus",
      thirdProduct: "Laughing Skull"
    };
  },
  created: function() {},
  methods: {}
};
</script>