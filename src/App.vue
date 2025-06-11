<script setup>
import { ref, onMounted } from 'vue'
import Card from './components/Card.vue'
import { svgCardIcons } from '@/assets/cards'

const side = ref(4)
const total = side.value * side.value

const icons = ref([])
const flippedCards = ref([])
const matchedCards = ref([])

const moves = ref(0)
const gameFinished = ref(false);

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

  moves.value++;
  card.flipped = true;
  flippedCards.value.push(card);

  if (flippedCards.value.length === 2) {
    const [first, second] = flippedCards.value;
    if (first.id === second.id) {
      first.matched = true;
      second.matched = true;
      flippedCards.value = [];
      if (icons.value.every(card => card.matched)) {
          setTimeout(() => {
          gameFinished.value = true;
              }, 600); // Немного задержки, чтобы анимация успела закончиться
         }
    } else {
      setTimeout(() => {
        first.flipped = false;
        second.flipped = false;
        flippedCards.value = [];
      }, 500);
    }
  }
}

function restartGame() {
  const shuffled = shuffleArray(svgCardIcons).slice(0, total);
  icons.value = shuffled.map(svg => ({
    id: svg.id,
    svg: svg.svg,
    flipped: false,
    matched: false,
  }));
  moves.value = 0;
  gameFinished.value = false;
}

</script>

<template>
  <div class="game-board flex justify-center items-center min-h-screen bg-gray-100 px-4">

    <!-- Карточки — показываются, пока игра не завершена -->
    <div
      v-if="!gameFinished"
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

    <!-- Статистика — появляется, когда игра завершена -->
    <transition name="fade-scale">
      <div v-if="gameFinished" class="game-summary text-center bg-white p-8 rounded-xl shadow-xl">
        <h2 class="text-2xl font-bold mb-4">🎉 Игра завершена!</h2>
        <p class="text-lg mb-4">Количество ходов: {{ moves }}</p>
        <button
          @click="restartGame"
          class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded-lg transition"
        >
          Сыграть ещё раз
        </button>
      </div>
    </transition>

  </div>
</template>




<style scoped>
/* tailwind уже задаёт gap, но можно усилить так */
.grid {
  display: grid;
}
</style>
