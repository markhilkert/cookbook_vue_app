<template>
  <div class="recipes-show">
    <img :src=" recipe.image_url " :alt="recipe.title ">
    <h1>{{ recipe.title }}</h1>
    <h4>Chef: {{ recipe.chef }}</h4>
    <h4>Prep Time: {{ recipe.formatted.prep_time }}</h4>
    <h3>Ingredients:</h3>

    <ul>
      <li v-for="ingredient in recipe.formatted.ingredients">{{ ingredient }}</li>
    </ul>

    <h3>Directions:</h3>

    <ol>
      <li v-for="direction in recipe.formatted.directions">{{ direction }}</li>
    </ol>

    <router-link class="btn btn-primary" :to=" '/recipes/' + recipe.id + '/edit'"> Edit </router-link>
    <button class="btn btn-primary" v-on:click="destroyRecipe()"> Delete </button>

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
                formatted:{
                  ingredients: [],
                  directions: [],
                  prep_time: "",
                  created_at: ""}
                }
    };
  },
  created: function() {
    axios.get("/api/recipes/" + this.$route.params.id )
      .then(response => {
        console.log(response.data);
        this.recipe = response.data;
      });
  },
  methods: {
    destroyRecipe: function() {
      axios.delete("/api/recipes/" + this.recipe.id)
      .then( response => {
        console.log("Sucess.", response.data)
        this.$router.push("/");
      });
    }
  }
}
</script>