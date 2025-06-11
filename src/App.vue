<script setup>
import { ref, onMounted } from 'vue'
import Card from './components/Card.vue'
import { svgCardIcons } from '@/assets/cards'

const side = ref(4)
const total = side.value * side.value

const icons = ref([])

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
