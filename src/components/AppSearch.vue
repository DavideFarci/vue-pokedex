<script setup>
import { ref, defineEmits } from 'vue';
const { log } = console;

const emit = defineEmits(['findPokemon']);
const pokemonToFind = defineModel();
const buttons = ref([
  {
    name: 'X',
    label: 'find',
    action: 'pokemonFind',
  },
  {
    name: 'Y',
    label: 'catch',
    action: 'onCatchPokemon',
  },
  {
    name: 'Z',
    label: 'switch',
    action: 'onToggleCatched',
  },
]);

const pokemonFind = () => {
  emit('findPokemon', pokemonToFind.value);

  pokemonToFind.value = '';
};
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
      <template v-for="(button, i) in buttons" :key="i">
        <div class="flex flex-col items-center gap-[1px]">
          <button
            @click="
              button.action === 'pokemonFind'
                ? pokemonFind()
                : $emit(button.action)
            "
            class="h-12 w-12 cursor-pointer rounded-full border border-b-[5px] border-red-950 bg-red-700 px-2 py-2 text-white transition-all active:translate-x-[1px] active:translate-y-[3px] active:border-b-[2px] active:brightness-90"
          >
            {{ button.name }}
          </button>
          <span class="text-xs font-semibold">{{ button.label }}</span>
        </div>
      </template>
    </div>
  </div>
</template>

<style scoped></style>
