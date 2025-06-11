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
  icons.value = shuffledIcons.slice(0, total)
})

function onCardClick(index) {
  // если уже угадана или уже открыта — игнор
  if (flippedCards.value.includes(index) || matchedCards.value.includes(index)) return;
  if (flippedCards.value.length === 2) return; // блокируем лишние клики

  flippedCards.value.push(index)

  if (flippedCards.value.length === 2) {
    const [first, second] = flippedCards.value
    if (icons.value[first].name === icons.value[second].name) {
      matchedCards.value.push(first, second)
      flippedCards.value = []
    } else {
      setTimeout(() => {
        flippedCards.value = []
      }, 1000)
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
        v-for="(icon, index) in icons"
        :key="index"
        :svg="icon"
        :isFlipped="flippedCards.includes(index) || matchedCards.includes(index)"
        :isMatched="matchedCards.includes(index)"
        @click="onCardClick(index)"
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
