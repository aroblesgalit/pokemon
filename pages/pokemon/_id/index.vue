<template>
    <div class="w-full">
        <div :class="[bgColor]">
            <h1>{{ char.species }}</h1>
            <h3>{{ char.num }}</h3>
            <img :src="char.sprite" :alt="char.species">
        </div>
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
                types {
                    name
                }
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
      
        pokemon.color = pokemon.color.toLowerCase();
        const name = pokemon.species;
        if (name.includes('nidoran')) {
            if (name[name.length - 1] == 'f') {
                pokemon.species = 'nidoran♀'
            } else {
                pokemon.species = 'nidoran♂'
            }
        }
        this.char = pokemon
    },
    computed: {
        bgColor() {
            return this.char.color == 'White' ?  
                `bg-stone-300` : (
                this.char.color == 'Brown' ?
                `bg-orange-300` : (
                this.char.color == 'Black' ?
                `bg-slate-400` :
                `bg-${this.char.color}-400`))
        }
    }
}
</script>

<style scoped>

</style>