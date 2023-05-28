<template>
  <div>
    <p v-for="char in pokemon.sort((a, b) => a.num - b.num)" :key="char.num">
      {{ char.num + ' ' + char.species }}
    </p>
  </div>
</template>

<script>
export default {
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
