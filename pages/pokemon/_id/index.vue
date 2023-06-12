<template>
    <div>
        {{ char.species }}
    </div>
</template>

<script>
export default {
    data() {
        return {
            char: {}
        }
    },
    async fetch() {
        const pokemon = await fetch('https://graphqlpokemon.favware.tech/v7', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          query: `
            {
              getPokemonByDexNumber(number: ${this.$route.params.id}) {
                num
                species
                sprite
                backSprite
                color
                height
                weight
                baseForme
                baseSpecies
                abilities {
                    first {
                        name
                        desc
                    }
                    second {
                        name
                        desc
                    }
                    special {
                        name
                        desc
                    }
                    hidden {
                        name
                        desc
                    }
                }
                baseStats {
                    attack
                    defense
                    hp
                    specialattack
                    specialdefense
                    speed   
                }
                evolutions {
                    species
                }
                preevolutions {
                    species
                }
              }
            }
          `
        })
      }).then(res => res.json())
        .then(res => JSON.parse(JSON.stringify(res.data.getPokemonByDexNumber)))
      
      this.char = pokemon
    }
}
</script>

<style scoped>

</style>