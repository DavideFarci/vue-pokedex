<script setup>
import axios from 'axios';
import { ref, watch } from 'vue';
import AppSearch from './components/AppSearch.vue';
import PokemonDetails from './components/PokemonDetails.vue';
import AppCatched from './components/AppCatched.vue';
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
  const searchedPokemon = pokemonToFind;

  if (!searchedPokemon) {
    loader.value = false;
    return;
  } else {
    try {
      const data = await axios.get(
        `https://pokeapi.co/api/v2/pokemon/${searchedPokemon}`,
      );
      let stats = [];

      data.data.stats.forEach((stat) => {
        if (!stats[stat.stat.name]) {
          const _stat = {
            name: stat.stat.name,
            value: stat.base_stat,
          };

          stats.push(_stat);
        }
      });

      pokemon.value = {
        name: data.data.forms[0].name,
        type: data.data.types[0].type.name,
        height: data.data.height,
        weight: data.data.weight,
        stats: stats,
        img_front: data.data.sprites.front_default,
        img_back: data.data.sprites.back_default,
      };

      loader.value = false;
      thumb.value = true;
      catched.value === true && (catched.value = false);
    } catch (error) {
      console.log('Errore durante la richiesta: ' + error.message);
      if (error.response.status && error.response.status == 404) {
        message.value = 'Pokemon non trovato';
        loader.value = false;
        pokemon.value = [];
      }
    }
  }
};

const toggleCatched = () => {
  catched.value = !catched.value;
};

const catchPokemon = () => {
  const match = pokemonsList.value.find(
    (obj) => obj.name === pokemon.value.name,
  );
  if (match) {
    message.value = 'Hai già catturato questo Pokemon';
    setTimeout(() => {
      message.value = '';
    }, 2000);
  } else {
    pokemonsList.value.push(pokemon.value);
    localStorage.setItem('pokemons', JSON.stringify(pokemonsList.value));
    catched.value === false && (catched.value = true);
  }
};

const updateList = (listValue) => {
  pokemonsList.value = JSON.parse(localStorage.getItem('pokemons'));
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
      class="mx-auto grid h-[40rem] max-w-6xl grid-cols-2 rounded-md border-[8px] border-slate-400/70 bg-gradient-to-r from-red-800 via-red-950 to-red-800 shadow-red-900"
      style="
        border-radius: 9px 9px 40px 40px;
        box-shadow: 0px 7px 0px 0px #450a0a;
      "
    >
      <div class="grid grid-rows-2">
        <!-- ricerca e anteprima  -->
        <div class="py-6">
          <div
            class="mx-auto h-full w-1/2 bg-slate-900"
            style="border-radius: 9px 9px 40px 9px"
          >
            <template v-if="pokemon && !loader">
              <img
                v-if="thumb"
                :src="pokemon.img_front"
                :alt="pokemon.name"
                class="h-full"
              />
              <img
                v-else
                :src="pokemon.img_back"
                :alt="pokemon.name"
                class="h-full"
              />
            </template>

            <!-- loader -->
            <div v-if="loader" class="flex h-full items-center justify-center">
              <div class="flex flex-row gap-2">
                <div
                  class="h-4 w-4 animate-bounce rounded-full bg-blue-700 [animation-delay:.7s]"
                ></div>
                <div
                  class="h-4 w-4 animate-bounce rounded-full bg-blue-700 [animation-delay:.3s]"
                ></div>
                <div
                  class="h-4 w-4 animate-bounce rounded-full bg-blue-700 [animation-delay:.7s]"
                ></div>
              </div>
            </div>
          </div>
        </div>

        <!-- ricerca  -->
        <AppSearch
          v-model="pokemonToFind"
          @findPokemon="getPokemon"
          @onCatchPokemon="catchPokemon"
        />
      </div>

      <div class="grid grid-rows-6 gap-4 p-4">
        <div class="row-span-1 flex items-center justify-center">
          <button
            @click="toggleCatched"
            class="mx-auto rounded-md bg-red-950 px-6 py-1 font-bold"
          >
            {{ !catched ? 'Catched Pokemons' : 'Info' }}
          </button>
        </div>

        <div class="row-span-5">
          <!-- dettagli pokemon -->
          <PokemonDetails
            v-if="!catched"
            :message="message"
            :pokemon="pokemon"
            :loader="loader"
          />
          <AppCatched
            v-else
            v-on:onGetPokemon="getPokemon"
            :onUpdateList="updateList"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
