<script >
import { RouterLink, RouterView } from 'vue-router'
import HeaderNav from './components/HeaderNav.vue';
export default {
  components: {
    HeaderNav
  },
  created(){
  }
}
</script>

<template>
  
  <header>
    <HeaderNav />
  </header>
  <div class="loader" id="loader">
    <img src="./assets/loading.gif" alt="">
    <p>Carregando</p>
  </div>

  <RouterView />
</template>

<style >
.loader {

  position: fixed;
  display: none;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  left: 0px;
  top: 0px;
  width: 100%;
  height: 100%;
  z-index: 9999;
  width: 100%;
  height: 100vh;
  transition: visibility 0s 2s, opacity 2s linear;

  background-color: #1A1A1A;
}



#app {
  overflow: hidden;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}

/* width */
::-webkit-scrollbar {
  width: 10px;
}

/* Track */
::-webkit-scrollbar-track {
  background: #f1f1f1;
}

/* Handle */
::-webkit-scrollbar-thumb {
  background: #242424;
}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
  background: #555;
}
</style>
