<template>
  <section>
    <pokemon-card 
      :pokemons="starters" 
      :selectedId="selectedId"
      @pokemonClicked="fetchEvolutions"
    />
    <pokemon-card 
      :pokemons="evolutions" 
    />
  </section>

</template>

<script>
import UiCard from "./components/card.vue";
import PokemonCard from "./components/pokemon_card.vue";
const api = "https://pokeapi.co/api/v2/pokemon";
const STARTER_IDS = [1,4,5]
export default {
  data:() => ({
    starters: [],
    evolutions: [],
    selectedId: null
  }),
  components: {
    UiCard,
    PokemonCard
  },
  methods: {
    async fetchData(ids) {
      const response = await Promise.all(ids.map(each_id => window.fetch(`${api}/${each_id}`)))
      const data     = await Promise.all(response.map(resp => resp.json()))
      const pokemon  = data.map(each_data => ({
        id: each_data.id,
        name: each_data.name,
        sprite:each_data.sprites.other["official-artwork"].front_default,
        types: each_data.types.map(each_types => each_types.type.name)
      }));
      return pokemon;
    },
    async fetchEvolutions(pokemon) {
      this.selectedId = pokemon.id
      const evolutions =await this.fetchData([pokemon.id + 1, pokemon.id + 2])
      this.evolutions = evolutions
    }
  },
  async created() {
    const starters = await this.fetchData(STARTER_IDS)
    this.starters = starters
  }
}
</script>

<style>
 * {
   margin: 0;
   padding: 0;
 }
 section {
   display: flex;
   flex-direction: column;
   align-items: center;
 }

</style>