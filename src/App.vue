  
<template>
  <v-app id="inspire">

    <v-app-bar
      app
      color="indigo"
      class="d-flex justify-center"
      dark
    >
      <v-toolbar-title>My Recipies</v-toolbar-title>
    </v-app-bar>

    <v-main>
      <v-container
        class="fill-height"
        :fluid="$vuetify.breakpoint.lgOnly"
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
                <v-row justify="end">
                <div class="mx-3 caption">
                  Posted {{ recipe.postDate }}
                </div>
                </v-row>
              </v-card-text>
            </v-card>
            
          </v-col>
        </v-row>
      </v-container>

      <v-dialog v-model="details"
        max-width="750"
        :fullscreen="$vuetify.breakpoint.xsOnly"
      >
        <recipe :recipe="selectedRecipe" :specials="specials" @close="details = false"></recipe>
      </v-dialog>

      <v-snackbar bottom color="error" v-model="errorBar">
        {{ errorMsg }}
      </v-snackbar>

    </v-main>
    <v-footer
      color="indigo"
      app
    >
      <span class="white--text">&copy; 2020</span>
    </v-footer>
  </v-app>
</template>

<script>
  import recipe from "./components/recipe.vue"

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
      errorBar: false,
      details: false,
      selectedRecipe: null,
    }),

    components: {
      recipe: recipe
    },

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


