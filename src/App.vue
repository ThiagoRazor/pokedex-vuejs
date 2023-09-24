<template>

  <header class="mainBox">
      <div>
          <router-link :to="{ name: 'PokemonList'}" class="logoPath" href="#">
            <img class="pokedexLogo" src="./assets/img/logo.png" alt="Pokedex Logo" style="height:90px;width:270px; vertical-align: middle;"/> 
            <img src="https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/showdown/25.gif" style="width:50px; height:60px; vertical-align: middle;" alt="Pikachu">
          </router-link>
      <div>
      </div>
    </div>
      
  </header>
  <main>
        <router-view></router-view>
  </main>

</template>


<style>

  .mainBox {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding-top:15px; 
  }



</style>