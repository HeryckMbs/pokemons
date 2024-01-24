<template>
    <div class="main-container">
        <Presentation :detalhesEspecie="detalhesEspecie" :pokemon="pokemon"></Presentation>

        <div class="infos">
            <div>
                <h4>About</h4>
                <div class="baseContainer">
                    <p>{{ descriptionPokemon }}</p>
                </div>
            </div>
            <div>
                <h4>Habilities</h4>
                <div class="baseContainer">
                    <ul class="habilities">
                        <li :key="index" v-for="(item, index) in pokemon.abilities">
                            <p>{{ transformaCamelCase(item.ability.name) }}</p>
                        </li>
                    </ul>

                </div>
            </div>

            <RowList :title="'Base Stats'" :arrayContent="baseStats">
            </RowList>

            <div class="combat">
                <GridDamage :arrayDamage="strongAgainst" :title="'Double damage To'"></GridDamage>
                <GridDamage :arrayDamage="weakAgainst" :title="'Double damage from'"></GridDamage>
            </div>


            <RowList :title="'Extra Info'" :arrayContent="extraInfo">
            </RowList>


            <div>
                <h4>Egg Groups</h4>
                <div class="baseContainer">
                    <ul class="habilities">
                        <li :key="index" v-for="(item, index) in detalhesEspecie.egg_groups">
                            <p>{{ transformaCamelCase(item.name) }}</p>
                        </li>
                    </ul>

                </div>
            </div>

        </div>
    </div>
</template>

<style >
.combat {
    display: flex;
    justify-content: space-between;
    margin: 3% auto;
}


.baseContainer {
    background-color: #1A1A1A;
    padding: 2%;
    margin-top: 10px;
    border-radius: 10px;
    height: 100%;
}



.links {
    width: 90%;
    margin: 0 auto;
    padding: 1%;
    justify-content: space-between;
    display: flex;
    gap: 2%;
}


.links a {
    background-color: #242424;
    padding: 1% 2%;
}



.infos {
    width: 70%;
}

ul {
    margin: 0;
    padding: 0;
    display: flex;
    justify-content: space-around;
}

li p {
    color: white;
    text-align: center;
    padding-right: 16px;
    text-decoration: none;
}
</style>

<script>
import GridDamage from './GridDamage.vue'
import RowList from './RowList.vue'
import Presentation from './Presentation.vue'
export default {
    components: {
        GridDamage,
        RowList,
        Presentation
    },
    props: {
        pokemon: Object,
        detalhesEspecie: Object,
        strongAgainst: Object,
        weakAgainst: Object,
    },
    data() {
        return {

        }
    },

    computed: {
        baseStats() {
            if (Object.keys(this.pokemon).length > 0) {
                return this.pokemon.stats.map(function (element) { return { label: element.stat.name, 'text': element.base_stat } });
            }
            return []
        },
        extraInfo() {
            if (Object.keys(this.detalhesEspecie).length > 0 && Object.keys(this.pokemon).length > 0) {

                return [
                    { label: 'Hapiness', text: this.detalhesEspecie.base_happiness ?? '-' },
                    { label: 'Capture Rate', text: this.detalhesEspecie.capture_rate ?? '-' },
                    { label: 'Color', text: this.detalhesEspecie.color.name ?? '-' },
                    { label: 'Base Experience', text: this.pokemon.base_experience ?? '-' },
                    
                ]

            }
            return []
        },
        generaPokemon() {
            if (this.detalhesEspecie.genera) {
                let genera = this.detalhesEspecie.genera.filter((element) => element.language.name == 'en')[0].genus
                return genera
            }
            return ''
        },
        descriptionPokemon() {
            if (this.detalhesEspecie.flavor_text_entries) {
                let description = this.detalhesEspecie.flavor_text_entries.filter((element) => element.language.name == 'en')[0].flavor_text
                return description
            }
            return ''
        },
    },
    methods: {

        transformaCamelCase(inputString) {
            if (inputString != undefined) {
                // Dividir a string em palavras usando o caractere "-"
                const palavras = inputString.split('-');

                // Capitalizar a primeira letra de cada palavra
                const palavrasCapitalizadas = palavras.map(palavra =>
                    palavra.charAt(0).toUpperCase() + palavra.slice(1)
                );

                // Unir as palavras com um espaço
                const resultado = palavrasCapitalizadas.join(' ');

                return resultado;
            }
            return ''
        },



    }
}
</script>