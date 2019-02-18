<template>              
  <div class="home"> 
    <h1>New Recipe</h1>
    <div>
      <div>
        Title: <input v-model="newRecipeTitle">
      </div>
      <div>
        Chef: <input v-model="newRecipeChef">
      </div>
      <div>
        Prep Time: <input v-model="newRecipePrepTime">
      </div>
      <div>
        Ingredients: <input v-model="newRecipeIngredients">
      </div>
      <div>
        Directions: <input v-model="newRecipeDirections">
      </div>
      <div>
        Image URL: <input v-model="newRecipeImageUrl">
      </div>
      <button v-on:click="createRecipe()">Create</button>
    </div>

    <h1>All Recipes</h1>
    <div v-for="recipe in recipes">

      <h2>{{ recipe.title }}</h2>
      <img v-bind:src="recipe.image_url" v-bind:alt="recipe.title">
      <p> Prep Time: {{ recipe.prep_time }} </p>
      <p> Ingredients: {{ recipe.ingredients }} </p>
      <p> Directions: {{ recipe.directions }} </p>
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
      newRecipeTitle: "",
      newRecipeChef: "",
      newRecipePrepTime: "",
      newRecipeIngredients: "",
      newRecipeDirections: "",
      newRecipeImageUrl: ""
    };
  },
  created: function() {           
    axios.get("http://localhost:3000/api/recipes")
      .then (response => {
        this.recipes = response.data;
      });
  },
  methods: {
    createRecipe: function() {
      console.log("Create the Recipe...")
      var params = {
                    title: this.newRecipeTitle,
                    chef: this.newRecipeChef,
                    prep_time: this.newRecipePrepTime,
                    ingredients: this.newRecipeIngredients,
                    directions: this.newRecipeDirections,
                    image_url: this.newRecipeImageUrl
                    };
      axios.post("/api/recipes", params)
        .then(response => {
          console.log("Success", response.data);
          this.recipes.push(response.data);
        });
    }
  }
};
</script>