<script >

export default {
 
}
</script>

<template>
  <main>
    <div class="container">
      <div class="presentation">
        <div class="logo">
          <img src="../assets/logo.png" alt="">
        </div>
        <p>Explore o fascinante mundo dos Pokémon no PokéDex Explorer, seu guia definitivo. Com informações abrangentes e
          ilustrações impressionantes, nossa plataforma intuitiva oferece detalhes sobre todas as espécies, desde as mais
          populares até as mais raras. Seja você um Treinador iniciante ou um amante dedicado, mergulhe conosco no
          universo Pokémon!</p>
        <router-link class="button-acess" to="/catalogo"> Acessar Pokédex </router-link>

      </div>
      <div class="art">
        <img src="../assets/pokemons.png" alt="">
      </div>
    </div>
  </main>
</template>

<style >
.button-acess {
  padding: 2% 4%;
  width: 30%;
  background-color: red;
  margin: 3% 0;
  border: none;
  border-radius: 10px;
  color: white;
  display: flex;
  justify-content: center;
  font-weight: bolder;
  box-shadow: 0 7px 15px rgba(255, 255, 255, 0.2);

}

.container {
  display: flex;
  justify-content: space-around;
  align-items: center;
  width: 95%;
  padding: 4%;
}



.art img {
  width: 120%;
}

.art {
  flex: 1;
  display: flex;
  justify-content: center;
}

.presentation {
  display: flex;
  flex: 1;
  flex-direction: column;
  height: 100%;
  justify-content: start;
}

.presentation p {
  margin-top: 5%;
  margin-bottom: 3%;
  color: white;
  max-width: 80%;
}

main {
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 10px;
}
</style>