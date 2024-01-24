<template>
    <div class="main-container evolucoesContainer">
        <h3>Evolutions</h3>
        <div class="evolucoes">
            <div class="evolucao-item" v-for="(item, key, index) in arvoreEvolucao" :key="index">
                    <PokemonCard style="max-width: 30%;" :pokemon="item" :key="item.name">
                    </PokemonCard>

                    <div class="" style="display: flex; flex-direction: column;">
                        <div v-for="(item, key, index) in linhasEvolucao" :key="index"
                        style="display: flex;flex-direction: row !important;" class="">
                        <div class="" style="display: flex ; align-items: center;justify-content: center; flex: 1;" v-for="(pokem, index, key) in item">
                            
                            <h1 v-if="index < item.length">></h1>
                            
                            <PokemonCard style="max-width: 90%;"  :pokemon="pokem" :key="pokem.name">
                            </PokemonCard>
                            
                        </div>
                    </div>
                </div>
            </div>

        </div>
    </div>
</template>

<style>
.evolucao-item {
    display: flex;
    align-items: center;
    justify-content: center;

}

.evolucoesContainer {
    display: flex;
    flex-direction: column;
    justify-content: center;
    margin-top: 3% !important;
    align-items: center;
}

.evolucoes {
    width: 80%;
    flex-wrap: wrap;

}
</style>

<script>

import PokemonCard from '../Home/PokemonCard.vue';

export default {
    components: { PokemonCard },
    props: {
        linhasEvolucao: Object,
        arvoreEvolucao: Object
    }
}
</script>