<template>
    <div class="w-full pokemon-detail">
        <div :class="[bgColor]" class="header">
            <div class="max-w-lg mx-auto text-center pt-5 relative">
                <h1 class="font-bold text-5xl text-white">{{ char.species }}</h1>
                <h3 class="font-bold text-lg text-white">#{{ char.num }}</h3>
                <img 
                    :src="char.sprite" 
                    :alt="char.species"
                    class="mx-auto relative left-0 right-0 scale-125"
                >
            </div>
        </div>
        <div class="max-w-lg mx-auto pt-10">
            <ul>
                <li>
                    <span>Height: </span><span>{{ char.height }} m</span>
                </li>
                <li>
                    <span>Weight: </span><span>{{ char.weight }} kg</span>
                </li>
                <li>
                    <span>Type: </span><span v-for="el in char.types">{{ el.name }}, </span>
                </li>
            </ul>
        </div>
        <div class="max-w-lg mx-auto pt-10">
            <ul>
                <li>
                    <span>Attack: </span><span>{{ char.baseStats && char.baseStats.attack }}</span>
                </li>
                <li>
                    <span>Defense: </span><span>{{ char.baseStats && char.baseStats.defense }} </span>
                </li>
                <li>
                    <span>HP: </span><span>{{ char.baseStats && char.baseStats.hp }}</span>
                </li>
                <li>
                    <span>Special Attack: </span><span>{{ char.baseStats && char.baseStats.specialattack }}</span>
                </li>
                <li>
                    <span>Special Defense: </span><span>{{ char.baseStats && char.baseStats.specialdefense }}</span>
                </li>
                <li>
                    <span>Speed: </span><span>{{ char.baseStats && char.baseStats.speed }}</span>
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
            return this.char.color == 'white' ?  
                `bg-stone-300` : (
                this.char.color == 'brown' ?
                `bg-orange-300` : (
                this.char.color == 'black' ?
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

.pokemon-detail h1 {
    text-transform: capitalize;
}
</style>