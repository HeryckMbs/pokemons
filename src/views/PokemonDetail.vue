<template>
    <div class="links">
        <a v-if="$route.params.id > 1" :href="`/pokemon/${parseInt($route.params.id) - 1}`">Previous</a>
        <a :href="`/pokemon/${parseInt($route.params.id) + 1}`">Next</a>
    </div>
    <PresentationComponent :pokemon="pokemon" :detalhesEspecie="detalhesEspecie" :strongAgainst="strongAgainst"
        :weakAgainst="weakAgainst"></PresentationComponent>
    <EvolutionComponent v-if="linhasEvolucao && arvoreEvolucao" :linhasEvolucao="linhasEvolucao" :arvoreEvolucao="arvoreEvolucao" />
    <GaleryComponent :images="pokemon['sprites']"></GaleryComponent>
</template>


<style >
h2,
h1,
h4,
h3,
h5,
h6 {
    color: #ED1E24;

}
</style>

<script>
import Api from '@/http/Api';
import ProgressBar from 'primevue/progressbar';
import PokemonCard from '@/components/Home/PokemonCard.vue';
import EvolutionComponent from '@/components/Details/EvolutionComponent.vue';
import GaleryComponent from '@/components/Details/GaleryComponent.vue'
import PresentationComponent from '@/components/Details/PresentationComponent.vue';
export default {

    components: {
        ProgressBar,
        PokemonCard,
        GaleryComponent,
        EvolutionComponent,
        PresentationComponent,
        PresentationComponent
    },

    data() {
        return {
            linhasEvolucao: {},
            arvoreEvolucao: {},
            pokemon: {},
            evolucao: {},
            detalhesEspecie: {},
            strongAgainst: {},
            weakAgainst: {},
        };
    },
    methods: {
        mountChain() {
            Api.callApi().get(`/pokemon/${this.evolucao.specie_initial.name}`).then(res => {

                this.arvoreEvolucao[0] = res.data;
            }).catch(error => { })

            for (let item in this.evolucao.evolutions) {

                let linhaEvolucaoPoke = this.evolucao.evolutions[item];
                this.linhasEvolucao[linhaEvolucaoPoke.line] = [];

                for (var pokemonIt of linhaEvolucaoPoke.evolutions) {
                    Api.callApi().get(`/pokemon/${pokemonIt.name}`).then(res => {
                        this.linhasEvolucao[linhaEvolucaoPoke.line].push(res.data)
                    }).catch(error => { })
                }

            }

        },
        mountEvolutionTree(tree) {
            let chain = tree.evolves_to;


            this.evolucao['specie_initial'] = tree.species;
            this.evolucao['evolutions'] = [];
            for (let item in chain) {
                let cadeia = chain[item];

                let evolutionLine = { line: item, evolutions: [] }

                while (cadeia.evolves_to.length > 0) {
                    evolutionLine.evolutions.push(cadeia.species)
                    cadeia = cadeia.evolves_to[0];
                }

                if (cadeia.evolves_to.length == 0) {
                    evolutionLine.evolutions.push(cadeia.species)

                }
                this.evolucao['evolutions'].push(evolutionLine)


            }


            this.mountChain();
        },
        filtrarNaoNulos(objeto) {
            return Object.entries(objeto)
                .filter(([chave, valor]) => valor !== null)
                .reduce((objFiltrado, [chave, valor]) => {
                    if (typeof valor === 'string') {
                        objFiltrado[chave] = valor;
                    }
                    return objFiltrado;
                }, {});
        },

        async getSpecieDetail() {
            console.log(!this.$store.state.loading)

            await Api.callApi().get(`/pokemon-species/${this.pokemon.id}`).then(response => {

                this.detalhesEspecie = response.data
                this.getEvolutionChain(response.data.evolution_chain.url);
                this.getDamageRelation();
            }).catch(error => {
            })



        },
        async getEvolutionChain(url) {
            await Api.callApi().get(url).then(response => {
                this.mountEvolutionTree(response.data.chain)
            }).catch(error => {
            })
        },

        async getDamageRelation() {

            for (let item of this.pokemon.types) {
                await Api.callApi().get(`${item.type.url}`).then(response => {

                    for (let damage of response.data.damage_relations.double_damage_from) {
                        if (this.weakAgainst[damage.name] == null) {
                            this.weakAgainst[damage.name] = 1;
                        } else {
                            this.weakAgainst[damage.name]++;
                        }


                    }
                    for (let damage of response.data.damage_relations.double_damage_to) {
                        if (this.strongAgainst[damage.name] == null) {
                            this.strongAgainst[damage.name] = 1;
                        } else {
                            this.strongAgainst[damage.name]++;
                        }

                    }

                }).catch(error => {
                })
            }

        }
    },
    created() {
        console.log('s')
        if (Object.keys(this.$store.state.pokemon_selecionado).length > 0) {
            this.pokemon = this.$store.state.pokemon_selecionado
            this.getSpecieDetail();
        } else {
            let id = this.$route.params.id;
            Api.callApi().get(`/pokemon/${id}`).then(res => {
                this.pokemon = res.data
                this.getSpecieDetail();

            }).catch(error => { })
        }
    }
}
</script>