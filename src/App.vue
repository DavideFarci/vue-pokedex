<script setup>
import axios from 'axios';
import { ref, watch } from 'vue';
import AppSearch from './components/AppSearch.vue';
import PokemonDetails from './components/PokemonDetails.vue';
import AppCatched from './components/AppCatched.vue';
import AppImgDisplay from './components/AppImgDisplay.vue';
const { log } = console;

const pokemon = ref(null);
const message = ref('');
const catched = ref(false);
const thumb = ref(false);
const pokemonToFind = ref('');
const pokemonsList = ref(
  localStorage.getItem('pokemons')
    ? JSON.parse(localStorage.getItem('pokemons'))
    : [],
);
const loader = ref(false);
let timeoutId = null;

const getPokemon = async (pokemonToFind) => {
  loader.value = true;
  message.value = '';

  const _searchedPokemon = pokemonToFind.trim().toLowerCase();
  const searchedPokemon = _searchedPokemon;

  if (!searchedPokemon) {
    loader.value = false;
    return;
  } else {
    try {
      const data = await axios.get(
        `https://pokeapi.co/api/v2/pokemon/${searchedPokemon}`,
      );
      const _pokemon = data.data;

      let stats = [];

      _pokemon.stats.forEach((stat) => {
        const _stat = {
          name: stat.stat.name,
          value: stat.base_stat,
        };

        stats.push(_stat);
      });

      pokemon.value = {
        id: _pokemon.id,
        name: _pokemon.forms[0].name,
        type: _pokemon.types[0].type.name,
        height: _pokemon.height,
        weight: _pokemon.weight,
        stats: stats,
        img_front: _pokemon.sprites.front_default,
        img_back: _pokemon.sprites.back_default,
      };

      loader.value = false;
      thumb.value = true;
      catched.value === true && (catched.value = false);
    } catch (error) {
      console.log('Errore durante la richiesta: ' + error.message);
      if (error.response.status && error.response.status == 404) {
        message.value = 'Pokemon not found';
        loader.value = false;
        pokemon.value = [];
      } else {
        message.value = 'An error occurred';
        loader.value = false;
        pokemon.value = [];
      }
    }
  }
};

const catchPokemon = () => {
  if (pokemon.value) {
    const _pokemonsList = JSON.parse(localStorage.getItem('pokemons'));
    const match = _pokemonsList.find((obj) => obj.id === pokemon.value.id);
    if (match) {
      message.value = 'You have already catched this Pokemon';
      setTimeout(() => {
        message.value = '';
      }, 2000);
    } else {
      pokemonsList.value.push(pokemon.value);
      localStorage.setItem('pokemons', JSON.stringify(pokemonsList.value));
      catched.value === false && (catched.value = true);
    }
  }
};

const removePokemon = (name) => {
  pokemonsList.value = pokemonsList.value.filter((pok) => pok.name !== name);
  localStorage.setItem('pokemons', JSON.stringify(pokemonsList.value));
};

const updateList = () => {
  pokemonsList.value = JSON.parse(localStorage.getItem('pokemons'));
};

const toggleCatched = () => {
  catched.value = !catched.value;
};

watch(thumb, () => {
  clearTimeout(timeoutId);
  timeoutId = setTimeout(() => {
    thumb.value = !thumb.value;
  }, 3000);
});
</script>

<template>
  <div class="p-6">
    <h1 class="py-12 text-center text-4xl font-bold">BooleDEX</h1>
    <div
      class="gameBoy mx-auto grid h-[40rem] max-w-6xl grid-cols-2 rounded-md border-b-[15px] border-red-900 bg-gradient-to-r from-red-800 from-40% via-orange-950 via-50% to-red-800 to-60% shadow-red-900"
      style=""
    >
      <!-- immagine e ricerca -->
      <div class="grid grid-rows-2">
        <AppImgDisplay
          :onPokemon="pokemon"
          :onThumb="thumb"
          :onLoader="loader"
        />

        <AppSearch
          v-model="pokemonToFind"
          @findPokemon="getPokemon"
          @onCatchPokemon="catchPokemon"
          @onToggleCatched="toggleCatched"
        />
      </div>

      <!-- dettagli o lista -->
      <div class="py-12 pl-24 pr-14">
        <PokemonDetails
          v-if="!catched"
          :message="message"
          :pokemon="pokemon"
          :loader="loader"
        />
        <AppCatched
          v-else
          @onGetPokemon="getPokemon"
          :on-pokemons-list="pokemonsList"
          @onRemovePokemon="removePokemon"
        />
      </div>
    </div>
  </div>
</template>

<style>
.gameBoy {
  border-radius: 9px 9px 40px 40px;
  box-shadow: 0px 7px 0px 0px #450a0a;
}

.border_special {
  border-radius: 9px 9px 40px 9px;
}
</style>
