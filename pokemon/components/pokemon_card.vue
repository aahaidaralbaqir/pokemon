<template>
  <div class="cards">
    <ui-card 
      v-for="(pokemon, index) in pokemons"
      :key="index"
      :starter="pokemon"
      :class="{opace: selectedId && pokemon.id != selectedId}"
      class="card"
      @click="click(pokemon)"
    >
      <template v-slot:title>
        {{ pokemon.name }} # {{ pokemon.id }}
      </template>
      <template v-slot:content>
        <img :src="pokemon.sprite" alt="">
      </template>
      <template v-slot:description>
          <div v-for="(type,index) in pokemon.types" :key="index">
            {{ type }}
          </div>
      </template>
     </ui-card >
  </div>
</template>

<script>
import Card from "./card.vue";
export default {
  props: {
    pokemons: {
      type: Array
    },
    selectedId: {
      type: Number
    }
  },
  components: {
    'ui-card': Card
  },
  methods: {
    click(pokemon) {
      this.$emit("pokemonClicked",pokemon)
    }
  }
}
</script>

<style scoped>
 .cards {
   display: flex;
   margin-top: 20px;
 }
 .opace {
   opacity: 0.5;
 }
 .card:hover {
   opacity: 1;
 }
 img {
    width: 100%;
  }
</style>