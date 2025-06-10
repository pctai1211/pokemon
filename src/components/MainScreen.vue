<script setup lang="ts">
import { ref } from 'vue';
import PlayScreen from './PlayScreen.vue';

export type Match = {
  cards: number;
  name: string;
  description: string;
};

const matches: Match[] = [
  {
    cards: 8,
    name: "Very Easy Match",
    description: "A simple match game with 8 cards.",
  },
  {
    cards: 16,
    name: "Easy Match",
    description: "A simple match game with 16 cards.",
  },
  {
    cards: 32,
    name: "Medium Match",
    description: "A medium match game with 32 cards.",
  },
  {
    cards: 64,
    name: "Hard Match",
    description: "A hard match game with 64 cards.",
  },
];

const selectedMatch = ref<Match | null>(null);

function selectMatch(match: Match) {
  selectedMatch.value = match;
}
function nextMatch() {
  if (selectedMatch.value?.cards === 64) { return; }
  const findIndex = matches.findIndex((m) => m.cards === selectedMatch.value?.cards);
  selectedMatch.value = matches[findIndex + 1];
}
</script>

<template>
  <div class="w-full pt-10">
    <div v-if="selectedMatch === null">
      <h5 class="text-2xl pb-4">Select a match type:</h5>
      <div class="flex flex-col gap-4">
        <div v-for="match in matches" :key="match.cards" @click="selectMatch(match)"
          class="p-4 border rounded-lg hover:border-green-500 cursor-pointer">
          <h6 class="text-lg font-semibold">{{ match.name }}</h6>
          <p>{{ match.description }}</p>
          <p>Cards: {{ match.cards }}</p>
        </div>
      </div>
    </div>
    <div v-else>
      <button @click="selectedMatch = null"
        class="bg-green-500 text-white px-4 py-1 rounded-lg block mb-4">Back</button>
      <PlayScreen :matchType="selectedMatch" :nextMatch="nextMatch" :key="selectedMatch.cards" />
    </div>
  </div>

</template>
