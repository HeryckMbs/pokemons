<template>
    <div class="item" @click="goToPokemonDetails($event, pokemon)">


        <img class="artPokemon" :src="pokemon.sprites.other['official-artwork'].front_default" alt="">
        <div class="">
            <div class="content">
                <h3>{{ captalizeName(pokemon.name) }}</h3>
                <h3 class="codPokemon">N° {{ zeroEsquerda(pokemon.id) }}</h3>
            </div>
            <div class="types">
                <div v-for="(item, index) in pokemon.types" :key="index" :class="['type', item.type.name]">
                    <img :src="`/src/assets/icons/${item.type.name}.png`" class="iconType" alt="">
                    {{ captalizeName(item.type.name) }}
                </div>
            </div>
        </div>

    </div>
</template>

<script>
import UtilsMixin from '@/mixins/utils'
export default {
    created() {
        console.log(this)
    },
    methods: {
        async goToPokemonDetails(event, pokemon) {
            this.$store.commit('setLoading')
            await this.$store.commit('setPokemon', pokemon)

            await this.$router.push({ path: '/pokemon/' + pokemon.id }).then(() => this.$router.go())
            this.$store.commit('unsetLoading')

        }
    },
    name: 'PokemonCard',
    props: {
        pokemon: Object
    },
    mixins: [UtilsMixin]
}
</script>

<style scoped>
h3 {
    color: white;
}

.item {
    grid-area: 'item';
    display: flex;
    flex-direction: column;
    justify-content: center;
    background-color: #303134;
    margin: 15px;
    box-shadow: 0 7px 15px rgba(0, 0, 0, 0.8);
    padding: 15px;
    border-radius: 10px;
    -webkit-transform: translateY(0);
    -moz-transform: translateY(0);
    -ms-transform: translateY(0);
    -o-transform: translateY(0);
    transform: translateY(0);
    -webkit-transition: 0.5s;
    -o-transition: 0.5s;
    -moz-transition: 0.5s;
    transition: 0.5s;
}

.item:hover {
    -webkit-transform: translateY(-16px);
    -moz-transform: translateY(-16px);
    -ms-transform: translateY(-16px);
    -o-transform: translateY(-16px);
    transform: translateY(-16px);
}

.content {

    padding: 5% 0;
}

.item a {
    display: flex;
    flex-wrap: nowrap;
    height: 100%;
    flex-direction: column;
}

.types {
    display: flex;
    flex-direction: row;
    gap: 10px;
    margin-bottom: 10px

}

.iconType{
    max-width: 30%;
    margin-right: 10px;
}

.type {
    margin-right: 5px;
    padding: 4% 8%;
    max-width: 40%;
    box-shadow: 0 3px 11px rgba(0, 0, 0, 0.8);
    font-size: 100%;
    border-radius: 10px;
    display: flex; justify-content: space-around;
}

.link {
    background-color: #121212;
    padding: 10px;
    border-radius: 10px;
    background-color: #550899;
    box-shadow: 0 7px 15px rgba(0, 0, 0, 0.8);
}

.link a {
    color: white;

}


.codPokemon {
    font-style: italic;
    font-weight: 100;
    color: white;

}


.artPokemon {
    max-height: 34vh;

    border-radius: 10px;
    background: #242424;
}

.content h2 {
    font-size: 20pt;
    color: white;
}

.content p {
    margin-top: 5px;
    font-size: 15pt;
}</style>