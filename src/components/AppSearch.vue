<script setup>
import { ref, defineEmits } from 'vue';
const { log } = console;

const pokemonToFind = defineModel();

const pokemonFind = () => {
  emit('findPokemon', pokemonToFind.value);

  pokemonToFind.value = '';
};

const emit = defineEmits(['findPokemon']);
</script>

<template>
  <div class="flex flex-col items-center p-12">
    <label for="catch" class="font-semibold">Find your Pokemon</label>
    <input
      v-model="pokemonToFind"
      @keydown.enter="pokemonFind"
      type="text"
      name="catch"
      id="catch"
      class="mb-5 rounded-md p-1 text-black"
    />

    <div class="flex-start flex gap-3">
      <div class="flex flex-col items-center gap-[1px]">
        <button
          @click="pokemonFind"
          class="h-12 w-12 cursor-pointer rounded-full border border-b-[5px] border-red-950 bg-red-700 px-2 py-2 text-white transition-all active:translate-x-[1px] active:translate-y-[3px] active:border-b-[2px] active:brightness-90"
        >
          X
        </button>
        <span class="text-xs font-semibold">FIND</span>
      </div>

      <div class="flex flex-col items-center gap-[1px]">
        <button
          @click="$emit('onCatchPokemon')"
          class="h-12 w-12 cursor-pointer rounded-full border border-b-[5px] border-red-950 bg-red-700 px-2 py-2 text-white transition-all active:translate-x-[1px] active:translate-y-[3px] active:border-b-[2px] active:brightness-90"
        >
          Y
        </button>
        <span class="text-xs font-semibold">CATCHED</span>
      </div>
    </div>
  </div>
</template>

<style scoped></style>
