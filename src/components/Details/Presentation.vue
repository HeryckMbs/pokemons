<template>
    <div class="presentation">
            <h2>#{{ pokemon.id }}</h2>
            <h1>{{ transformaCamelCase(pokemon.name) }}</h1>
            <div style="display: flex; justify-content: center;">
                <h6 class="">{{ generaPokemon }}</h6>
            </div>
            <div class="image">

                <img v-if="pokemon.sprites != undefined" :src="pokemon.sprites.other['official-artwork'].front_default"
                    alt="">

            </div>

            <div class="typeList">
                <div :class="['badge', item.type.name]" :key="idx" v-for="(item, idx) in pokemon.types">
                    <img :src="`/src/assets/icons/${item.type.name}.png`" alt="">
                    <p> {{ transformaCamelCase(item.type.name) }}</p>
                </div>
                <div class="badge water" v-if="detalhesEspecie.is_baby">
                    <p>Baby</p>
                </div>
                <div class="badge dark" v-if="detalhesEspecie.is_legendary">
                    <p>Legendary</p>
                </div>
                <div class="badge dark" v-if="detalhesEspecie.is_mythical">
                    <p>Mythical</p>
                </div>
            </div>

            <p v-if="Object.keys(detalhesEspecie).length > 0 && detalhesEspecie.habitat != undefined"><b>Habitat</b> {{
                transformaCamelCase(detalhesEspecie.habitat.name) }} </p>

            <p><strong>Height:</strong> {{ pokemon.height / 10 }} m</p>
            <p><b>Weight:</b> {{ pokemon.weight / 10 }} Kg</p>
            <p>Male: {{ getGender.male }}%</p>
            <p>Female: {{ getGender.female }}%</p>

        </div>
</template>

<style >

.badge img {
    width: 15% !important;
    /* background-color: transparent; */
}
.image {
    margin-top: 10%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
}


.image img {
    width: 90%;
}

.badge {
    margin-right: 5px;
    padding: 2% 4%;
    box-shadow: 0 3px 11px rgba(0, 0, 0, 0.8);
    font-size: 100%;
    border-radius: 10px;
    display: flex;
    justify-content: space-around;
    align-items: center;
}
.typeList {

margin-top: 9%;
display: flex;
justify-content: space-around;
padding: 0 8%;
gap: 10%;
}
.presentation .badge {
    max-width: 50% !important;

}
.presentation {
    width: 20%;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    background-color: #1A1A1A;
    border-radius: 10px;
    padding: 1%;
}
</style>
<script>
export default{
    props:{
        detalhesEspecie:Object,
        pokemon:Object,
    },
    computed: {
     
        generaPokemon() {
            if (this.detalhesEspecie.genera) {
                let genera = this.detalhesEspecie.genera.filter((element) => element.language.name == 'en')[0].genus
                return genera
            }
            return ''
        },
        getGender() {
            let genderRate = this.detalhesEspecie.gender_rate;
            switch (genderRate) {
                case 0:
                    return { 'male': 100, 'female': 0 };
                case 1:
                    return { 'male': 87.5, 'female': 12.5 };
                case 2:
                    return { 'male': 75, 'female': 25 };
                case 3:
                    return { 'male': 62.5, 'female': 37.5 };
                case 4:
                    return { 'male': 50, 'female': 50 };
                case 5:
                    return { 'male': 37.5, 'female': 62.5 };
                case 6:
                    return { 'male': 25, 'female': 75 }
                case 7:
                    return { 'male': 12.5, 'female': 87.5 };
                case 8:
                    return { 'male': 0, 'female': 100 };
                default:
                    return { 'make': 0 };
            }
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