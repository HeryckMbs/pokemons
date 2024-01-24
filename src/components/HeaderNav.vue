<template>
    <div class="nav">
        <img src="../assets/pokebola.png" alt="">
        <div class="nav-items">
            <router-link to="/">Home</router-link>
            <router-link to="/catalogo">Pokedex</router-link>

            
        </div>
    </div>
</template>

<script>
export default {
    data(){
        return {

        }
    }
}
</script>

<style scoped>
.nav{
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 100%;
    padding: 1%;
    background-color: transparent;
}

.nav img{
    height: 7vh;
}

.nav a{
    color: white;
    margin-right: 5px;
    font-size: 18px;
}
</style>