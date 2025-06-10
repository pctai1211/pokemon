<script setup lang="ts">
import { defineProps, onMounted, ref } from 'vue';
import type { Match } from './MainScreen.vue';
import type { Card as ICard } from './Card.vue';
import Card from './Card.vue';
const { matchType, nextMatch } = defineProps<{ matchType: Match, nextMatch: () => void; }>();
function shuffleArray<T>(array: T[]): T[] {
    const newArray = [...array];
    for (let i = newArray.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [newArray[i], newArray[j]] = [newArray[j], newArray[i]];
    }
    return newArray;
}
const selectedCards = ref<ICard[]>([]);
const cardRefs = ref<Array<InstanceType<typeof Card> | null>>([]);

const cards = ref<ICard[]>([]);

function generateCards() {
    const pokemons = Array.from({ length: matchType.cards / 2 }, (_, i) => i + 1);
    const shuffledPokemons = shuffleArray([...pokemons, ...pokemons]);
    cards.value = shuffledPokemons.map((pokemon, index) => ({
        id: pokemon,
        disabled: false,
        index,
    }));
}
onMounted(generateCards)
const onClickedCard = (card: ICard) => {
    if (selectedCards.value.length < 2) {
        selectedCards.value.push(card);
    }
    if (selectedCards.value.length === 2) {
        if (selectedCards.value[0].id === selectedCards.value[1].id) {
            console.log(selectedCards.value)
            cards.value = cards.value.map(c => {
                if (c.id === selectedCards.value[0].id) {
                    return { ...c, disabled: c.id === selectedCards.value[0].id }
                }
                return c
            });
            selectedCards.value = [];
            const finished = cards.value.every(card => card.disabled);
            if (finished) {
                setTimeout(() => {
                    nextMatch()
                    alert('Congratulations! You have completed the game!');
                    selectedCards.value = [];
                    cardRefs.value.forEach(cardRef => cardRef?.onFlipBackCard());
                }, 500);
            }
        } else {
            console.log(selectedCards.value, cardRefs.value)
            setTimeout(() => {
                selectedCards.value.forEach(({ index }) => {
                    cardRefs.value[index]?.onFlipBackCard();
                });
                selectedCards.value = [];
            }, 500);
        }
    }
};

</script>
<template>
    <div class="flex justify-center items-center">
        <div class="grid gap-1"
            :style="{ gridTemplateColumns: `repeat(${Math.sqrt(matchType.cards)}, minmax(0, 56px))` }">
            <div v-for="(card) in cards" :key="card.id + '-' + card.index"
                class="flex items-center justify-center w-14 h-14">
                <Card :card="card" :onClickCard="onClickedCard"
                    :ref="(el) => cardRefs[card.index] = el as InstanceType<typeof Card> | null" />
            </div>
        </div>
    </div>
</template>
