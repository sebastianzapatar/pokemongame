<template>
  <h1 v-if="!pokemon">Cargando</h1>
  <div v-if="pokemon">
    <h1>Who's that Pokemon???</h1>
  <PokemonPictureVue :pokemonId="pokemon.id" 
  :showPokemon="this.showPokemon" 
 />
  <PokemonPageVue :pokemons="pokemonArr"
  @selection="checkAnswer"/>
  {{ mensaje }}
  </div>
  <button @click="newGame">Reset</button>
</template>

<script>
import getPokemonOptions from './helpers/getPokemonOptions'

import PokemonPageVue from './pages/components/PokemonPage.vue'
import PokemonPictureVue from './pages/components/PokemonPicture.vue'


export default {
  name: 'App',
  components: {
    PokemonPageVue,
    PokemonPictureVue
  },
  data(){
    return {
      pokemonArr:[],
      pokemon:null,
      showPokemon:false,
      mensaje:''
    }
  },
  methods:{
    async mixPokemonArray(){
      this.pokemonArr=await getPokemonOptions();
      const rndInt=Math.floor(Math.random()*4);
      this.pokemon=this.pokemonArr[rndInt];
    },
    checkAnswer(pokemonId){
      this.showPokemon=true
      this.mensaje=pokemonId==this.pokemon.id ?"Correct" +" "+this.pokemon.name+"!!":"Wrong"+" "+this.pokemon.name+"!!";
    },
    newGame(){
      this.pokemonArr=[];
      this.pokemon=null;
      this.showPokemon=false;
      this.mensaje='';
      this.mixPokemonArray()
    }
  },
  mounted(){
    this.mixPokemonArray()
  }
}
</script>

<style>

</style>
