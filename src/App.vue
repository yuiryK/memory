<script setup>
import { ref, onMounted } from 'vue'
import Card from './components/Card.vue'
import { svgCardIcons } from '@/assets/cards'

const side = ref(4)
const total = side.value * side.value

const icons = ref([])
const flippedCards = ref([])
const matchedCards = ref([])

function shuffleArray(array) {
  const shuffled = array.slice(); // копия массива
  for (let i = shuffled.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [shuffled[i], shuffled[j]] = [shuffled[j], shuffled[i]];
  }
  return shuffled;
}

onMounted(() => {
  // При монтировании — просто берём первые 16 SVG
  const shuffledIcons = shuffleArray(svgCardIcons);
  icons.value = shuffledIcons.slice(0, total).map((svg, index) => ({
    id: svg.id,
    svg: svg.svg,
    flipped: false,
    matched: false
  }));
})


function handleCardClick(card) {
  if (card.flipped || card.matched || flippedCards.value.length === 2) return;

  card.flipped = true;
  flippedCards.value.push(card);

  if (flippedCards.value.length === 2) {
    const [first, second] = flippedCards.value;
    if (first.id === second.id) {
      first.matched = true;
      second.matched = true;
      flippedCards.value = [];
    } else {
      setTimeout(() => {
        first.flipped = false;
        second.flipped = false;
        flippedCards.value = [];
      }, 500);
    }
  }
}


</script>

<template>
  <div class="flex justify-center items-center min-h-screen bg-gray-100 px-4">
    <div
      class="grid gap-x-6 gap-y-6 p-6 bg-white shadow-xl rounded-xl"
      :style="{
        gridTemplateColumns: `repeat(${side}, minmax(100px, 1fr))`,
        gap: '24px',
        maxWidth: `${side * 140}px`,
        width: '100%',
        margin: '0 auto'
      }"
    >
<Card
  v-for="(card, index) in icons"
  :key="index"
  :svg="card"
  :flipped="card.flipped"
  :matched="card.matched"
  @click="handleCardClick(card)"
/>
    </div>
  </div>
</template>



<style scoped>
/* tailwind уже задаёт gap, но можно усилить так */
.grid {
  display: grid;
}
</style>
