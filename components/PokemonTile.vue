<template>
    <nuxt-link :to="`/pokemon/${char.num}`">
        <div 
            :id="char.num"
            :class="[bgColor]"
            class="pokemon-tile relative shadow p-4 flex flex-col items-center justify-end gap-4 h-full rounded-2xl"
        >
            <span class="absolute text-sm font-bold text-white/75">{{ char.num }}</span>
            <div class="sprite relative w-full">
                <img :src="char.sprite" alt="" class="relative z-10 mx-auto" />
                <div class="absolute bottom-0 left-0 right-0 mx-auto text-center bg-white/50 z-0"></div>
            </div>
            <p class="capitalize">{{ charSpecies }}</p>
        </div>
    </nuxt-link>
</template>

<script>
export default {
    name: 'PokemonTile',
    props: ['char'],
    computed: {
        bgColor() {
            return this.char.color == 'White' ?  
                `bg-stone-300` : (
                this.char.color == 'Brown' ?
                `bg-orange-300` : (
                this.char.color == 'Black' ?
                `bg-slate-400` :
                `bg-${this.char.color.toLowerCase()}-400`))
        },
        charSpecies() {
            const name = this.char.species;
            return name.includes('nidoran') ? 
            ('nidoran' + (name[name.length - 1] == 'f' ? '♀' : '♂')) :
            name 
        }
    }
}
</script>

<style scoped>
.pokemon-tile > span {
    left: 10px;
    top: 10px;
}

.pokemon-tile > .sprite > div {
    height: 80px;
    width: 80px;
    border-radius: 50%;
}
</style>