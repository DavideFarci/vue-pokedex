<script setup>
defineProps({
  onPokemon: Object,
  onThumb: Boolean,
  onLoader: Boolean,
});
</script>

<template>
  <div class="row-span-2 px-12 py-8">
    <div
      class="border_special h-full border-8 border-amber-900 bg-red-900 py-4"
    >
      <div class="border_special mx-auto h-full w-2/3 bg-slate-900">
        <template v-if="onPokemon && !onLoader">
          <img
            v-if="onThumb"
            :src="onPokemon.img_front"
            :alt="onPokemon.name"
            class="h-full"
          />
          <img
            v-else
            :src="onPokemon.img_back"
            :alt="onPokemon.name"
            class="h-full"
          />
        </template>

        <!-- loader -->
        <div v-if="onLoader" class="flex h-full items-center justify-center">
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
  </div>
</template>

<style scoped></style>
