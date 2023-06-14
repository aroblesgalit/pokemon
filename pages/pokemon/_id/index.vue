<template>
    <div class="w-full pokemon-detail">
        <div :class="[bgColor]" class="header">
            <div class="max-w-3xl mx-auto text-center pt-5 relative">
                <h1 class="font-bold text-5xl text-white">{{ char.species }}</h1>
                <h3 class="font-bold text-lg text-white">#{{ char.num }}</h3>
                <img 
                    :src="char.sprite" 
                    :alt="char.species"
                    class="mx-auto relative left-0 right-0 scale-125"
                >
            </div>
        </div>
        <div class="max-w-3xl mx-auto pt-5">
            <ul>
                <li>
                    <span>height: </span><span>{{ char.height }}</span>
                </li>
                <li>
                    <span>weight: </span><span>{{ char.weight }}</span>
                </li>
                <li>
                    <span>type: </span><span v-for="el in char.types">{{ el.name}}</span>
                </li>
            </ul>
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
.pokemon-detail > .header > div > img {
    bottom: -20px;
    /* width: 100px; */
}
</style>