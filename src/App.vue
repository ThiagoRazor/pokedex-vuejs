<template>

  <header class="mainBox">
      <div>
          <a class="logoPath" href="./">
            <img class="pokedexLogo" src="./assets/logo.png" alt="Pokedex-Logo" style="height:110px;width:300px;"/> 
            <img src="./assets/pikachu.gif" style="width:70px; height:80px;" alt="Pikachu">
          </a>
      <div>
      </div>
    </div>
      
  </header>
  <main>
        <PokemonList/>
        
  </main>

</template>

<script setup>
  import PokemonList from './components/PokemonList.vue';

</script>

<style>

  .mainBox {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    gap:20px;
    padding-top:60px; 
  }



</style>