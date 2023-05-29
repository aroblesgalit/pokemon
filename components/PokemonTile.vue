<template>
    <div 
        :id="char.id"
        class="shadow p-4 flex flex-col items-center justify-end gap-4"
    >
      <img :src="char.sprite" alt="" />
      <p>
        {{ char.num + ' ' + char.species }}
      </p>
    </div>
</template>

<script>
export default {
    name: 'PokemonTile',
    props: ['char']
}
</script>

<style scoped>

</style>