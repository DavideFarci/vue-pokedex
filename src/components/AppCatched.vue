<script setup>
import { ref } from 'vue';
const { log } = console;

const pokemonsList = ref(
  localStorage.getItem('pokemons')
    ? JSON.parse(localStorage.getItem('pokemons'))
    : [],
);

const emitPokemon = defineEmits(['onGetPokemon']);

const removePokemon = (name) => {
  const result = pokemonsList.value.filter((pok) => pok.name !== name);
  pokemonsList.value = result;
  localStorage.setItem('pokemons', JSON.stringify(pokemonsList.value));
  $emit('onUpdateList');
};
</script>

<template>
  <div class="border_special h-full border-8 border-red-900 bg-slate-900 p-4">
    <ul>
      <li
        v-for="(pokemon, i) in pokemonsList"
        :key="i"
        class="flex justify-between"
      >
        <button
          class="font-semibold uppercase hover:font-bold"
          @click="$emit('onGetPokemon', pokemon.name)"
        >
          {{ pokemon.name }}
        </button>
        <button @click="removePokemon(pokemon.name)">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="16"
            height="16"
            fill="currentColor"
            class="bi bi-x-circle-fill"
            viewBox="0 0 16 16"
          >
            <path
              d="M16 8A8 8 0 1 1 0 8a8 8 0 0 1 16 0M5.354 4.646a.5.5 0 1 0-.708.708L7.293 8l-2.647 2.646a.5.5 0 0 0 .708.708L8 8.707l2.646 2.647a.5.5 0 0 0 .708-.708L8.707 8l2.647-2.646a.5.5 0 0 0-.708-.708L8 7.293z"
            />
          </svg>
        </button>
      </li>
    </ul>
  </div>
</template>

<style></style>
