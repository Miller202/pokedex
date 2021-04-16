<template>
  <div id="app">
    <figure id="logo">
    <img src="./assets/Pokedex-Logo.png" class="sizelogo"/>
    </figure>
    <div class="column is-half is-offset-one-quarter">
      <input type="text" class="input is-rounded" placeholder="buscar pokémon pelo nome" v-model="busca">
      <button class="button is-medium is-fullwidth is-warning is-light" id="BuscaBtn" @click="buscar()">
        Buscar</button>
      <div v-for="(poke, index) in filteredPokemons" :key="poke.url">
        <Pokemon :name="poke.name" :url="poke.url" :num="index+1"/>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import Pokemon from './components/Pokemon';
export default {
  name: 'App',
  data () {
    return {
      pokemons: [],
      filteredPokemons: [],
      busca: ''
    }
  },
  created(){
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res =>{
      console.log("lista de pokemons");
      this.pokemons = res.data.results;
      this.filteredPokemons = res.data.results;
    })
  },
  components: {
    Pokemon
  },
  methods: {
    buscar: function(){
      this.filteredPokemons = this.pokemons;
      if(this.busca == '' || this.busca == ' '){
        this.filteredPokemons = this.pokemons;
      }else{
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name.includes(this.busca.toLowerCase()))
      }
    }
  },
  computed: {
    /*
    resultadoBusca: function(){
      if(this.busca == '' || this.busca == ' '){
        return this.pokemons;
      }else{
        return this.pokemons.filter(pokemon => pokemon.name == this.busca)
      }
    }
    */
  }
}
</script>

<style>

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#logo {
  display: block;
  margin-left: auto;
  margin-right: auto
}

#BuscaBtn {
  margin-top: 2%;
}

</style>
