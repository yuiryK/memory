<script setup>
import Card from './components/Card.vue'
import UserInput from './components/UserInput.vue'
import { ref } from 'vue';
//const totalCards = ref(0);
const svgCardIcons = ref([]);
const side = ref(1);
function handleGeneration(total) {
    side.value = Math.sqrt(total);
    icons.value = Array.from({ length: total }, () => Math.floor(Math.random() * 100)); // рандом
    icons.value = svgCardIcons.value.slice(0, total); // если нужно использовать svgCardIcons
    }
</script>

<template>
 <UserInput @generate="handleGeneration" />
  <div class="grid" :style="{ gridTemplateColumns: `repeat(${side}, 1fr)` }">
    <Card />
   </div>
</template>

<style scoped>
.grid {
  display: grid;
  gap: 8px;
}
</style>
