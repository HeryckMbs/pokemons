<template>
    <div class="containerCatalogo">
        <GridPokemon></GridPokemon>
    </div>
</template>

<script>
import GridPokemon from '@/components/Home/GridPokemon.vue';
export default{
    components:{
        GridPokemon
    }
}
</script>

<style scoped>
.containerCatalogo{
    margin: 1% 10%;
}
</style>