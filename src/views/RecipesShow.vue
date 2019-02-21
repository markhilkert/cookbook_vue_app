<template>
  <div class="recipes-show">
    <h1> {{ recipe.title}} </h1>
    <h4> Chef: {{recipe.chef}} </h4>
    <h4> Prep Time: {{ recipe.prep_time }} </h4>
    <p> Ingredients: {{ recipe.ingredients }} </p>
    <p> Directions: {{ recipe.directions }} </p>
  </div>
</template>

<style>
  
</style>

<script>
  var axios = require('axios');

  export default {
    data: function() {
      return {
        recipe: {
                id: "",
                title: "",
                chef: "",
                ingredients: "",
                directions: "",
                prep_time: "",
                image_url: "",
                formatted: {
                  ingredients: [],
                  directions: [],
                  prep_time: "",
                  created_at: ""
                }
              }
      };
    },
    created: function() {
      axios.get('/api/recipes/' + this.$route.params.id)
        .then( response => {
        console.log(response.data);
        this.recipe = response.data;
      });
    },
    methods: {}
  }
</script>