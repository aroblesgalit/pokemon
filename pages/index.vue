<template>
  <div class="grid grid-cols-3 md:grid-cols-6 gap-4 p-10">
    <PokemonTile 
      v-for="char in pokemon.sort((a, b) => a.num - b.num)" 
      :key="char.num"
      :char="char"
    />
    <div class="hidden bg-green-400 bg-red-400 bg-blue-400 bg-stone-300 bg-orange-300 bg-yellow-400 bg-purple-400 bg-pink-400 bg-gray-400 bg-slate-400"></div>
  </div>
</template>

<script>
import PokemonTile from '../components/PokemonTile.vue';

export default {
  components: {
    PokemonTile
  },
  data() {
    return {
      pokemon: []
    }
  },
  async created() {
    // const config = {
    //   headers: {
    //     'Content-Type': 'application/json'
    //   }
    // }
    for (let i = 1; i <= 150; i++) {
      fetch('https://graphqlpokemon.favware.tech/v7', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          query: `
            {
              getPokemonByDexNumber(number: ${i}) {
                num
                species
                sprite
                color
              }
            }
          `
        })
      }).then(res => res.json())
        .then(res => this.pokemon.push(JSON.parse(JSON.stringify(res.data.getPokemonByDexNumber))))
    }
    // this.pokemon = this.pokemon[0] + this.pokemon[1]
    console.log(this.pokemon)
  },
  head() {
    return {
      title: 'Pokemon',
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: 'A collection of Pokemon'
        }
      ]
    }
  }
}
</script>
