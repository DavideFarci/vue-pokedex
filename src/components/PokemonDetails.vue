<script setup>
defineProps({
  message: String,
  pokemon: Object,
  loader: Boolean,
});
</script>

<template>
  <div
    v-if="!message && !loader"
    class="mx-auto h-full w-3/4 bg-slate-900 p-4 text-white"
    style="border-radius: 9px 9px 40px 9px"
  >
    <!-- generalità -->
    <ul>
      <li>
        <h2 class="py-2 text-center text-lg font-bold uppercase">info</h2>
      </li>
      <li>
        Name:
        <span v-if="pokemon" class="ml-2 font-bold uppercase">{{
          pokemon.name
        }}</span>
      </li>
      <li>
        Type:
        <span v-if="pokemon" class="ml-2 font-bold uppercase">{{
          pokemon.type
        }}</span>
      </li>
      <li>
        Height:
        <span v-if="pokemon" class="ml-2 font-bold uppercase"
          >{{ pokemon.height }}''</span
        >
      </li>
      <li>
        Weight:
        <span v-if="pokemon" class="ml-2 font-bold uppercase"
          >{{ pokemon.weight }} lbs</span
        >
      </li>
    </ul>

    <!-- statistiche -->
    <ul>
      <h2 class="py-2 text-center text-lg font-bold uppercase">statistics</h2>
      <template v-if="pokemon">
        <li v-for="(stat, i) in pokemon.stats" :key="i">
          <h4 class="py-1 font-bold">{{ stat.name }}</h4>
          <div class="h-2 rounded-sm border border-white bg-transparent">
            <div
              class="h-full rounded-sm bg-white"
              :style="{ width: stat.value / 2 + '%' }"
            ></div>
          </div>
        </li>
      </template>
    </ul>
  </div>
  <div
    v-else-if="message && !loader"
    class="mx-auto h-full w-3/4 bg-slate-900 p-4 text-white"
    style="border-radius: 9px 9px 40px 9px"
  >
    <h3 class="mt-12 text-center font-semibold uppercase">{{ message }}</h3>
  </div>
  <!-- loader -->
  <div
    v-else-if="loader && !message"
    class="flex h-full items-center justify-center"
  >
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
</template>

<style></style>
