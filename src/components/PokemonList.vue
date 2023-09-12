<template>

    <section class="pokeModal">
        <div class="searchBarBox">
            <input class="searchBar" v-model="searchPokemon" type="text" placeholder="Digite o nome ou ID... ">
            <img src="../assets/lupa.png" style="width:20px; height: 20px; background-color: aliceblue; cursor:pointer;"/>
        </div>
        <div class="wrapper">
            <div class="wrappedContent">
            <div class="elementArea" v-for="(pokemon, index) in filteredPokemons" :key="pokemon.name">
                <div>
                    <div :style="{backgroundColor: typePallete[types1[index]]}" class="pokemonFrame">
                        <img style="height:80px; width:70px;" :src="getImg(pokemon.url)" :alt="pokemon.name"/>
                    </div>
                    <div class="pokemonTextArea" >
                        <div>ID: {{ getId(pokemon.url) }}</div>
                        <p>{{ capitalizeFirstLetter(pokemon.name) }}</p>
                    </div>
                </div>
                <div class="pokemonTypeArea">
                    <div v-if="types1[index]"  :style="{backgroundColor: typePallete[types1[index]]}" class="typeBox">
                        {{capitalizeFirstLetter(types1[index])}}
                    </div>
                    <div v-if="types2[index]"  :style="{backgroundColor: typePallete[types2[index]]}" class="typeBox">
                        {{capitalizeFirstLetter(types2[index])}}
                    </div>
                </div>
            </div>

        </div>
        </div>

    </section>
</template>


<script>
    import axios from 'axios';

    export default {
        data(){
            return {
                info:[],
                filteredPokemons:[],
                types1:[],
                types2:[],
                endpoints:[],
                loading: true,
                errored: false,
                typePallete: {
                    'grass'   :  "#9BCC50",
                    'poison'  :  "#B97FC9",
                    'fire'    :  "#FD7D24",
                    'flying'  :  "#3DC7EF",
                    'water'   :  "#4592C4",
                    'bug'     :  "#729F3F",
                    'normal'  :  "#A4ACAF",
                    'electric' : "#EED535",
                    'ground'  :  "#AB9842",
                    'fairy'   :  "#FDB9E9",
                    'fighting':  "#D56723",
                    'psychic' :  "#F366B9",
                    'rock'    :  "#A38C21",
                    'steel'   :  "#9EB7B8",
                    'ice'     :  "#51C4E7",
                    'ghost'   :  "#7B62A3",
                    'dragon'  :  "#F16E57",
                },
                searchPokemon:''
            }
            
        },

        mounted(){
            this.getPokemons()
        },

        methods: {
            getPokemons() {
                axios
                    .get("https://pokeapi.co/api/v2/pokemon?limit=151")
                    .then(response => {
                        this.info = response.data.results
                        this.filteredPokemons = this.info
                        
                        for(var i = 0; i < this.filteredPokemons.length; i++){

                            this.endpoints.push(this.filteredPokemons[i].url)
                        }
                        

                        axios.all(this.endpoints.map((endpoint) => axios.get(endpoint)))
                            .then((data) => {
                            
                                for(var index = 0; index < this.filteredPokemons.length; index++){

                                    
                                        this.types1.push(data[index].data.types[0].type.name)
                                        
                                        if(data[index].data.types[1] !== undefined){
                                            this.types2.push(data[index].data.types[1].type.name )
                                        } else{
                                            this.types2.push('')
                                        }
                                    }
                            })
                            
                            
                        })


            },

            getId(url){
                return url.split('/').at(6)
            },
            getImg(url){
                const id = this.getId(url)
                
                return `https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/versions/generation-v/black-white/animated/${id}.gif`
            },
            capitalizeFirstLetter(name){
                if(name !== ''){
                
                const capitalizedFirst = name[0].toUpperCase();
                const rest = name.slice(1);
                return capitalizedFirst + rest;

                } else {
                    return ''
                }
            },
            
            
        },
        watch: {
            searchPokemon(newQuery){

                this.endpoints = []
                this.types1 = []
                this.types2 = []

                const query = newQuery.toLowerCase();
                this.filteredPokemons = this.info.filter(pokemon =>
                    pokemon.name.includes(query) ||
                    pokemon.url.split('/').at(6).includes(query)
                )


                for(var i = 0; i < this.filteredPokemons.length; i++){
                
                    this.endpoints.push(this.filteredPokemons[i].url)

                }

                axios.all(this.endpoints.map((endpoint) => axios.get(endpoint)))
                            .then((data) => {
                            
                                for(var index = 0; index < this.filteredPokemons.length; index++){

                                    
                                        this.types1.push(data[index].data.types[0].type.name)
                                        
                                        if(data[index].data.types[1] !== undefined){
                                            this.types2.push(data[index].data.types[1].type.name )
                                        } else{
                                            this.types2.push('')
                                        }
                                    }
                            })

            }
        }

    }

</script>



<style >

    .pokeModal {
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        margin-top:60px;
        gap:30px;
    }

    .searchBarBox{
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: aliceblue;
    border-radius:10px;
    height:30px;
    width:280px;
    padding:5px;
    margin-top:40px;
  }
  
  .searchBar {
    width: 100%;
    font-size: 16px;
    outline: none;
    background-color: transparent;
    border: 0;
  }


    .wrapper {
        width:600px;
        height:600px;
        overflow-y:auto;
        background-color: rgba(38, 76, 115, 0.459);
        border-radius: 20px;
    }

    .wrapper::-webkit-scrollbar{
        width:0px;
    }

    .wrappedContent{
        display: grid;
        grid-template-columns: repeat(3, 1fr);
    }

    .elementArea{
        background-color: rgb(10, 20, 30);
        margin:auto;
        margin-top:20px;
        margin-bottom:20px;
        padding-top:10px;
        height:200px;
        width: 160px;
        border: 1px solid rgb(70, 110, 155);
        border-radius: 10%;
        cursor:pointer;
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    .pokemonTextArea{
        width:inherit;
        padding:5px 10px;
        font-weight: bold;
        color:rgb(254, 254, 254);
    }

    .pokemonTypeArea{
        display: flex;
        flex-direction: row;
        justify-content: center;
        align-items: center;
        gap:5px;
    }


    .typeBox {
        background-color: #555f63;
        color:white;
        font-weight: bold;
        font-size: 14px;
        width:65px;
        height:20px;
        border-radius: 4px;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .pokemonFrame{
        border: 1px solid rgb(70, 110, 155);
        width: 100px;
        height: 100px;
        border-radius: 50%;
        display: flex;
        justify-content: center;
        align-items: center;
        object-fit:fill;
    }
</style>
