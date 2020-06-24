
  
<template>
  <v-card> 
      <v-card-title class="justify-space-between"> 
        {{ recipe.title }}
        <v-btn v-if="$vuetify.breakpoint.xsOnly" icon @click="$emit('close')"><v-icon>close</v-icon></v-btn>
      </v-card-title>
      
    
    <v-card-text>
      <v-img
        class="white--text align-end mb-3"
        height="200px"
        :src="require('../../public' + recipe.images.medium)"
      >
    </v-img>
      <p>
      <span class="font-weight-medium">{{ recipe.description }}</span> <br>
      <span class="pr-3"><v-icon>group</v-icon> <strong>{{ recipe.servings }}</strong> Servings</span>
      <span class="pr-3"><v-icon>schedule</v-icon> <strong>{{ recipe.servings }}</strong> min Preptime</span>
      <span class="pr-3"><v-icon>access_alarm</v-icon> <strong>{{ recipe.servings }}</strong> min Cooktime</span>
      </p>

      <h3>Ingrediants</h3>
      <ul>
        <li v-for="item in recipe.ingredients" :key="item.uuid">
           <strong>{{ item.amount }}</strong> {{ item.measurement }} {{ item.name }} 
           <span v-for="(special, index) in getSpecials(item)" :key="special.uuid" class="pb-3">
             <span v-if="index === 0"><br></span>
              <v-chip small> {{ special.type.toUpperCase()}}: {{ special.title }} </v-chip> <br>
              <span class="font-italic caption"> {{ special.text }}</span>
           </span>
        </li>
      </ul>
    </v-card-text>
  </v-card>
</template>

<script>

  export default {
    props: {
      recipe: { type: Object },
      specials: { type: Array },
    },
    data: () => ({
      
    }),

    methods: {
      getSpecials(ingredient) {
        return this.specials.filter(s => s.ingredientId === ingredient.uuid)
      },
    },

    mounted() {

    }
  }
</script>


