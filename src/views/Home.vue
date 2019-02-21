<template>              
  <div class="home"> 
    <h1>All Recipes</h1>
    <div v-for="recipe in recipes">
      <h2>{{ recipe.title }}</h2>
      <router-link v-bind:to="'/recipes/' + recipe.id">
        <img v-bind:src="recipe.image_url" v-bind:alt="recipe.title">
      </router-link>
    </div>
  </div>
</template>

<style>
img{
  width: 250px;
}      
</style>

<script>
var axios = require('axios');
export default {
  data: function() {
    return {
      recipes: [],
      currentRecipe: {}
    };
  },
  created: function() {           
    axios.get("http://localhost:3000/api/recipes")
      .then (response => {
        this.recipes = response.data;
      });
  },
  methods: {
    showRecipe: function(inputRecipe) {
      if (this.currentRecipe === inputRecipe) {
        this.currentRecipe = {};
      } else {
        this.currentRecipe = inputRecipe;
      }
    },
    updateRecipe: function(inputRecipe) {
      var params = {
                    title: inputRecipe.title,
                    chef: inputRecipe.chef,
                    prep_time: inputRecipe.prep_time,
                    ingredients: inputRecipe.ingredients,
                    directions: inputRecipe.directions,
                    image_url: inputRecipe.image_url
                    };
      axios.patch("/api/recipes/" + inputRecipe.id, params)
      .then( response => {
        console.log("Recipe Successfully Updated", response.data);
        inputRecipe = response.data;
      });
    },
    destroyRecipe: function(inputRecipe) {
      axios.delete("/api/recipes/" + inputRecipe.id)
      .then( response => {
        console.log("Sucess.", response.data)
        var index = this.recipes.indexOf(inputRecipe);  // Recipes are unique, finds the only one.
        this.recipes.splice(index,1);       // starts at index, removes only one element.
      });
    }
  }
};
</script>