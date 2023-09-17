<template>

  <header class="mainBox">
      <div>
          <a class="logoPath" href="./">
            <img class="pokedexLogo" src="./assets/logo.png" alt="Pokedex-Logo" style="height:90px;width:270px; vertical-align: middle;"/> 
            <img src="./assets/pikachu.gif" style="width:50px; height:60px; vertical-align: middle;" alt="Pikachu">
          </a>
      <div>
      </div>
    </div>
      
  </header>
  <main>
        
        <PokemonList :currentPage="currentPage" @totalPagesUpdated="updateTotalPages"/>
        <Pagination :currentPage="currentPage" :totalPages="totalPages" @updatePage="updatePage"/> 
  </main>

</template>

<script >
  import Pagination from './components/Pagination.vue';
  import PokemonList from './components/PokemonList.vue';

  export default{
    components: {
      Pagination, 
      PokemonList
    },

    data(){
      return {
        currentPage: 1,
        totalPages: 1
      }
    },
    methods:{
      
      updateTotalPages(totalPages){
        this.totalPages = totalPages;
      },
      updatePage(newPage){
        this.currentPage = newPage;
      }
    }
  }
</script>

<style>

  .mainBox {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding-top:15px; 
  }



</style>