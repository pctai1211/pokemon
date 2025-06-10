<script setup lang="ts">
import { defineProps, ref, defineExpose } from 'vue';
export type Card = {
    id: number;
    disabled: boolean;
    index: number;
};
const { card, onClickCard } = defineProps<{ card: Card, onClickCard: (card: Card) => void; }>();
const isShow = ref<boolean>(false)
const toogleShow = () => {
    console.log(card)
    if (card.disabled) return
    isShow.value = !isShow.value
    if (isShow.value) {
        onClickCard(card);
    }
}
const onFlipBackCard = () => {
    isShow.value = false
}
console.log(isShow.value)
defineExpose({
    onFlipBackCard,
});
</script>

<template>
    <div class="relative w-full h-full transition-transform duration-500 transform-style preserve-3d"
        :class="{ 'rotate-y-180': isShow, 'disabled:': card.disabled }" @click="toogleShow">
        <div class="absolute w-full h-full bg-gray-300 rounded-xl backface-hidden"></div>
        <div
            class="absolute w-full h-full bg-white rounded-xl backface-hidden transform rotate-y-180 flex items-center justify-center">
            <img :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${card.id}.png`"
                alt="Pokemon Card" class="w-16 h-16" />
        </div>
    </div>
</template>
<style scoped>
.perspective {
    perspective: 1000px;
}

.preserve-3d {
    transform-style: preserve-3d;
}

.backface-hidden {
    backface-visibility: hidden;
}

.rotate-y-180 {
    transform: rotateY(180deg);
}
</style>