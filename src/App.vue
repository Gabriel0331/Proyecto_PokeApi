<template>
  <header>
    <title>Pokedex - Gabriel Cardona</title>
  </header>
  <main>
    <img alt="Vue logo" src="https://upload.wikimedia.org/wikipedia/commons/9/98/International_Pok%C3%A9mon_logo.svg">
    <h2>Gabriel Cardona Garcés</h2>
    <PokeSearch :apiUrl="apiUrl" @setPokemonUrl="setPokemonUrl" />
    <div class="poke-container" id="poke-container"></div>
    <PokeDetail v-if="showDetail" :pokemonUrl="pokemonUrl" :imageUrl="imageUrl" @closeDetail="closeDetail" />
  </main>
</template>
  
  
<script>

import PokeDetail from "./PokeDetail.vue";
import PokeSearch from "./PokeSearch.vue";

export default {
  props: [
    'apiUrl'
  ],
  data: () => {
    return {
      searchvalue: '',
      imageUrl: "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/",
      apiUrl: "https://pokeapi.co/api/v2/pokemon/",
      pokemonUrl: "",
      showDetail: false,
      url: ""
    };
  },
  components: {
    PokeSearch,
    PokeDetail
  },
  methods: {
    setPokemonUrl(url) { 
      this.pokemonUrl = url;
      this.showDetail = true;
    },
    closeDetail() {
      this.pokemonUrl = "";
      this.showDetail = false;
    }
  }
};

const buscar = (url) => {
    alert(url)
}

/*const buscar = async (url) => {
  $emit('setPokemonUrl', url);
}

const setPokemonUrl = () => {
  alert("p")
  this.pokemonUrl = url;
  this.showDetail = true;
}*/

const pokemon_count = 50
const colors = {
  fire: '#FDDFDF',
  grass: '#DEFDE0',
  electric: '#FCF7DE',
  water: '#DEF3FD',
  ground: '#f4e4da',
  rock: '#d5d5d4',
  fairy: '#fceaff',
  poison: '#98d7a5',
  bug: '#f8d5a5',
  dragon: '#97b3e6',
  psychic: '#eaeda1',
  flying: '#F5F5F5',
  fighting: '#E6E0D4',
  normal: '#F5F5F5'
}

const main_types = Object.keys(colors)

const fetchPokemons = async () => { // 
  for (let i = 1; i <= pokemon_count; i++) {
    await getPokemon(i) // 
  }
}

const getPokemon = async (id) => { // 
  const url = 'https://pokeapi.co/api/v2/pokemon/' + id
  const res = await fetch(url) // 
  const data = await res.json() // 
  createPokemonCard(data, url)
  console.log(data)
}

const createPokemonCard = (pokemon, url) => {

  const name = pokemon.name[0].toUpperCase() + pokemon.name.slice(1)
  const id = pokemon.id.toString().padStart(3, '0')
  const poke_types = pokemon.types.map(type => type.type.name)
  const type = main_types.find(type => poke_types.indexOf(type) > -1)
  const color = colors[type]

  const poke_container = document.getElementById('poke-container')
  const pokemonEl = document.createElement('div')
  pokemonEl.id = id
  pokemonEl.classList.add('pokemon')
  pokemonEl.style.backgroundColor = color
  pokemonEl.style.borderRadius = "10px"
  pokemonEl.style.boxShadow = "0 3px 15px rgba(100, 100, 100, 0.5)"
  pokemonEl.style.margin = "10px"
  pokemonEl.style.padding = "20px"
  pokemonEl.style.textAlign = "center"

  //pokemonEl.click = setPokemonUrl

  const rutaCompleta = 'https://pokeapi.co/api/v2/pokemon/' + pokemon.id

  pokemonEl.addEventListener('click', function(){
    buscar(rutaCompleta);
  });

  const pokemonInnerHTML = `   
        <div class="img-container" style="background-color: rgba(255, 255, 255, 0.6); border-radius: 50%; width: 120px; height: 120px; text-align: center;">
          <img style="max-width: 90%; margin-top: 20px;" src="${pokemon.sprites.front_default}" alt="${pokemon.name}" >
        </div>
        <div class="info" style="margin-top: 20px;">
          <span class="number" style="background-color: rgba(0, 0, 0, 0.1); padding: 5px 10px; border-radius: 10px; font-size: 0.8em;">${id}</span>
          <h3 class="name" style="margin: 15px 0 7px; letter-spacing: 1px;">${name}</h3>
          <small class="type">Tipo: <span>${type}</span></small><br>
        </div>
        
      `
  pokemonEl.innerHTML = pokemonInnerHTML

  poke_container.appendChild(pokemonEl)

 /* const button = document.querySelector("button.alert");

  button.addEventListener(
    "click", () => {
      setPokemonUrl("https://pokeapi.co/api/v2/pokemon/1")
  });*/
  
}
fetchPokemons()





</script>
  
<style lang="scss" scoped>
@import url('https://fonts.googleapis.com/css?family=Lato:300,400&display=swap');
@import url("https://use.fontawesome.com/releases/v5.8.1/css/all.css");

* {
  box-sizing: border-box;
}

main {
  background: #efefbb;
  background: linear-gradient(to right, #d4d3dd, #efefbb);
  font-family: 'Lato', sans-serif;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin: 0;
}

h1 {
  letter-spacing: 2px;
}

.poke-container {
  display: flex;
  flex-wrap: wrap;
  align-items: space-between;
  justify-content: center;
  margin: 0 auto;
  max-width: 1200px;
}
</style>