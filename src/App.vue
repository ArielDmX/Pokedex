<template>
  <div class="pokedex">
    <img src="../public/img/pokedex.png">
    <div>
      <input class="input__search" id="form" type="text" placeholder="Name" v-model="busca">
      <div v-for="(poke) in filteredPokemons" :key="poke.url">
        <Pokemon :name="poke.name" :url="poke.url"/>
      </div>
      <button class="buttons" @click="buscar">Buscar</button>
    </div> 
  </div>
</template>

<script>

import axios from 'axios';
import Pokemon from './components/Pokemon'
export default {
  name: 'App',
  data(){
    return{
      pokemons:[],
      filteredPokemons: [],
      busca: ''
    }
  },
  created: function(){
    axios.get("https://pokeapi.co/api/v2/pokemon?limit=151&offset=0").then(res => {
      this.pokemons = res.data.results;
    })
  },
  components:{
    Pokemon
  },
  methods:{
    buscar: function(){
      if(this.busca == '' || this.busca == ' '){
        this.filteredPokemons = null;
      }else{
        this.filteredPokemons = this.pokemons.filter(pokemon => pokemon.name == this.busca);
      }
    }
  },
  computed:{
    resultadoBusca: function(){
      if(this.busca == ''|| this.busca == ' '){
        return;
      }else{
        return this.pokemons.filter(pokemon => pokemon.name == this.busca)
      }
    }
  }
}
</script>

<style>
.pokedex {
  width: 100%;
  max-width: 425px;
}
#form {
  position: absolute;
  width: 65%;
  top: 67%;
  left: 13.5%;
}
.input__search {
  width: 35%;
  padding: 2%;
  outline: none;
  border: 2px solid #333;
  border-radius: 5px;
  font-weight: 600;
  color: #3a444d;
  font-size: clamp(8px, 5vw, 1rem);
  box-shadow: -3px 4px 0 #888, -5px 7px 0 #333
}
.buttons {
  position: absolute;
  bottom: 18%;
  left: 16.5%;
  width: 60%;
  display: flex;
  border-radius: 5.5px;
  font-size: clamp(8px, 5vw, 1rem);
  font-weight: 650;
  color: white;
  background-color: #444;
  box-shadow: -2px 3px 0 #222, -4px 6px 0 #000;
}
</style>
