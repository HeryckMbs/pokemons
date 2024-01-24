<template>
    <button class="showFilters" @click="showFilters = !showFilters">Mostrar filtros</button>
    <Transition>

        <div v-if="showFilters" class="filtros">
            <div class="filterBox">
                <h1>Pesquisa</h1>
                <input type="text" name="" class="filterField" v-model="pesquisa" id="">
            </div>
            <div class="order">
                <label for="">Ordenar por</label>
                <select name="" @change="orderPokemons()" v-model="order" id="">
                    <option value="nomeCrescente">Nome - Crescente</option>
                    <option value="nomeDecrescente">Nome - Decrescente</option>
                    <option value="numeroCrescente">Número - Crescente</option>
                    <option value="numeroDecrescente">Número - Decrescente</option>
                </select>
            </div>
            <div class=" gridTypes">
                <div :class="[item, 'typeFilter']" @click="selectTypeFilter($event, item)"
                    v-for="(item, idx) in types">
                  {{
                        item.toLowerCase() }}</div>
            </div>

            <button class=" primary-button" @click="filterPokemon()">Pesquisar</button>

        </div>
    </Transition>
    <div class="grid">

        <PokemonCard class="item" :pokemon="pokemon" v-for="pokemon in pokemon_filtrados" :key="pokemon.id">
        </PokemonCard>

    </div>
    <div class="morePokemons" >
        <button style="margin: 0 auto" class="primary-button" @click="loadPokemons();">Carregar mais Pokemons</button>
    </div>
</template>

<style scoped>

.morePokemons{
    width: 100%; display: flex;
}
.morePokemons button


.showFilters{
    margin: 10%;
    
}


/* we will explain what these classes do next! */
.v-enter-active,
.v-leave-active {
    transition: opacity 0.3s ease;
}

.v-enter-from,
.v-leave-to {
    opacity: 0;
}

.typeFilter {
    opacity: 0.5;
    padding: 10px;
    display: flex;
    justify-content: center;
    font-size: 3vh;
}

.primary-button {
    padding: 1% 2%;
  
  background-color: red;
  margin: 3% 0;
  border: none;
  border-radius: 10px;
  color: white;
  display: flex;
  justify-content: center;
  font-weight: bolder;
  
}

.order {
    display: flex;
    flex-direction: column;
    margin: 2% auto;
}

.order select {
    border-radius: 10px;
    border: none;
    padding: 10px;
}

.filterBox {
    margin: 2% 0;
    width: 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
}

.filterField {
    width: 100%;
    margin-top: 10px;
    padding: 0.5%;
    border-radius: 10px;
    border: 0;
}

h1 {
    margin: 0 !important;
}

.grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
}

.gridTypes {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(90px, 1fr));
    gap: 10px;


}

.filtros {
    padding: 0 2%;
}
</style>

<script >
import Api from '/src/http/Api.js';
import PokemonCard from './PokemonCard.vue'
export default {
    components: { PokemonCard },
    created() {
        this.pokemon_filtrados = this.$store.state.pokemons_carregados;
    },

    data() {
        return {
            next_url: '',
            pokemon_filtrados: [],
            pesquisa: '',
            types: [],
            typesSelecteds: [],
            showFilters: false,
            order: ''
        }
    },
    methods: {
        selectTypeFilter(event, item) {
            if (!this.typesSelecteds.includes(item)) {
                this.typesSelecteds.push(item)
                event.srcElement.style.boxShadow = '0 3px 11px rgba(255, 255, 255, 0.8)'
                event.srcElement.style.opacity = '1'

            } else {
                this.typesSelecteds = this.typesSelecteds.filter((element) => element != item)
                event.srcElement.style.boxShadow = '0 3px 11px rgba(0, 0, 0, 0.8)'
                event.srcElement.style.opacity = '0.8'

            }


        },
        orderDecrescente(a, b) {
            console.log([a<b,a>b])
            if (a > b) {
                return -1;
            } else if (a < b) {
                return 1;
            } else {
                return 0;
            }
        },
        orderCrescente(a, b) {
            if (a < b) {
                return -1;
            } else if (a > b) {
                return 1;
            } else {
                return 0;
            }
        },
        filterPokemon() {
            console.log('oi')
            if (this.pesquisa == '' && this.typesSelecteds.length == 0) {
                this.pokemon_filtrados = this.$store.state.pokemons_carregados;
            } else {
                this.pokemon_filtrados = this.$store.state.pokemons_carregados.filter((element) => {
                    if (this.typesSelecteds.length == 0) {
                        return true
                    } else {
                        for (var type of element.types) {
                            if (this.typesSelecteds.includes(type.type.name)) {
                                return true
                            }
                        }
                        return false;
                    }


                }).filter((element) => this.pesquisa != '' ? element.name.includes(this.pesquisa.toLowerCase()) : true)


            }
        },

        orderPokemons() {
            console.log(this.order)
            if (this.order == 'nomeCrescente') {
                this.pokemon_filtrados = this.pokemon_filtrados.sort((a, b) => this.orderCrescente(a.name, b.name))
            }
            if (this.order == 'numeroCrescente') {
                this.pokemon_filtrados = this.pokemon_filtrados.sort((a, b) => this.orderCrescente(a.id, b.id))
            }
            if (this.order == 'nomeDecrescente') {
                this.pokemon_filtrados = this.pokemon_filtrados.sort((a, b) => this.orderDecrescente(a.nome, b.nome))
            }
            if (this.order == 'numeroDecrescente') {
                this.pokemon_filtrados = this.pokemon_filtrados.sort((a, b) => this.orderDecrescente(a.id, b.id))
            }
        },
        async loadPokemons() {
            if (!this.$store.state.loading) {
                this.$store.commit('setLoading')


                await Api.callApi().get(this.next_url != '' ? this.next_url : '/pokemon').then(response => {
                    this.next_url = response.data.next;
                    for (let item of response.data.results) {
                        Api.callApi().get(`/pokemon/${item.name}`).then(res => {
                            this.$store.commit('storePokemons', res.data)
                            this.pokemon_filtrados = this.$store.state.pokemons_carregados;

                        }).catch(error => { })
                    }
                    this.$store.commit('unsetLoading')

                }).catch(error => {
                    this.$store.commit('unsetLoading')

                })


            }


        }
    },
    mounted() {
        // window.addEventListener('scroll', () => {
        //     if (this.$route.name == 'catalogo') {
        //         const { scrollTop, scrollHeight, clientHeight } = document.documentElement;

        //         if (scrollTop + clientHeight >= scrollHeight - 5) {
        //             this.
        //         }
        //     }

        // });

    },
    beforeMount() {
        if (this.$store.state.pokemons_carregados.length == 0) {
            this.loadPokemons()
        }
        Api.callApi().get('/type/').then(response => {
            for (let item of response.data.results) {
                this.types.push(item.name)
            }
            this.$store.commit('unsetLoading')

        }).catch(error => {
            this.$store.commit('unsetLoading')

        })
    }
}
</script>