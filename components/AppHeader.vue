<template>
    <header class="flex justify-between items-center mb-4 border-b border-solid border-slate-400">
        <img class="logo" src="https://data.whicdn.com/images/92472243/original.gif" alt="Pokemon">
        <ul class="flex">
            <li>
                <nuxt-link to="/" class="inline-block py-1 px-4 mr-2">Home</nuxt-link>
            </li>
        </ul>
    </header>
</template>

<script>
export default {
    name: 'AppHeader'
}
</script>

<style scoped>
.logo {
    width: 200px;
    max-width: 80%;
}
</style>