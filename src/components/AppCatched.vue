<script setup>
import { ref, defineEmits } from 'vue';
const { log } = console;
defineProps({
  onPokemonsList: Array,
});
const emit = defineEmits(['onGetPokemon', 'onRemovePokemon']);
</script>

<template>
  <div class="border_special h-full border-8 border-red-900 bg-slate-900 p-4">
    <ul>
      <li>
        <h2 class="mb-2 py-2 text-center text-lg font-bold uppercase">
          pokemons catched
        </h2>
      </li>

      <li
        v-for="(pokemon, i) in onPokemonsList"
        :key="i"
        class="flex justify-between"
      >
        <button
          class="font-semibold uppercase hover:font-bold"
          @click="emit('onGetPokemon', pokemon.name)"
        >
          {{ pokemon.name }}
        </button>
        <button @click="emit('onRemovePokemon', pokemon.name)">
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
