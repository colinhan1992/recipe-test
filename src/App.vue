
  
<template>
  <v-app id="inspire">

    <v-app-bar
      app
      color="indigo"
      dark
    >
      <v-app-bar-nav-icon @click.stop="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>My Recipies</v-toolbar-title>
    </v-app-bar>

    <v-main>
      <v-container
        class="fill-height"
      >
        <v-row
          align="center"
          justify="center"
        >
          <v-col v-for="recipe in recipes" :key="recipe.uuid"
            :xl="3"
            :lg="4"
            :md="6"
            :xs="12"
            class="d-flex justify-center"
          >
            <v-card max-width="400" tabindex="0" @click="openRecipeDetails(recipe)">
              <v-img
                class="white--text align-end"
                height="200px"
                :src="require('../public' + recipe.images.medium)"
              >
              </v-img>
              <v-card-title>{{ recipe.title }}</v-card-title>
              
              <v-card-text>
                <p>
                {{ recipe.description }} 

                </p>
              </v-card-text>
            </v-card>
            
          </v-col>
        </v-row>
      </v-container>
    </v-main>
    <v-footer
      color="indigo"
      app
    >
      <span class="white--text">&copy; 2019</span>
    </v-footer>
  </v-app>
</template>

<script>
  const axios = require('axios');

  export default {
    props: {
      source: String,
    },
    data: () => ({
      drawer: null,
      recipes: [],
      specials: [],
      errorMsg: '',
    }),

    methods: {
      load() {
        this.loadRecipes()
        this.loadSpecials()
      },

      loadRecipes() {
        axios.get('http://localhost:3001/recipes')
        .then(response => {
          this.recipes = response.data
        })
        .catch(e => {
          this.errorMsg = 'There was an error getting the recipes'
          this.errorBar = true
          console.log(e)
        })
      },

      loadSpecials() {
        axios.get('http://localhost:3001/specials')
        .then(response => {
          this.specials = response.data
        })
        .catch(e => {
          this.errorMsg = 'There was an error getting the specials'
          this.errorBar = true
          console.log(e)
        })
      },

      openRecipeDetails(recipe) {
        this.selectedRecipe = recipe
        this.details = true
      }
    },

    mounted() {
      this.load()
    }
  }
</script>


