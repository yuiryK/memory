<script setup>
import { ref } from 'vue'

const props = defineProps({
  svg: Object
})

const flipped = ref(false)

function flipCard() {
  flipped.value = !flipped.value
}
</script>

<template>
  <div class="card" @click="flipCard">
    <div class="card-inner" :class="{ flipped }">
      <!-- Front side (icon) -->
      <div class="card-face card-front">
        <!-- Example placeholder, can be image or text -->
        ❓
      </div>
      <!-- Back side (cover or pattern) -->
      <div class="card-face card-back" v-html="svg.svg" />

    </div>
  </div>
</template>

<style scoped>
.card {
  width: 100px;
  height: 100px;
  perspective: 800px;
  cursor: pointer;
}

.card-inner {
  width: 100%;
  height: 100%;
  transition: transform 0.5s;
  transform-style: preserve-3d;
  position: relative;
}

.card-inner.flipped {
  transform: rotateY(180deg);
}

.card-face {
  position: absolute;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  border-radius: 12px;
  display: grid;
  place-items: center;
  border: 2px solid #ccc;
  background-color: #f8f8f8;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
}

.card-front{
  width: 80px;
  height: 80px;
}

.card-back {
  transform: rotateY(180deg);
  font-size: 32px;
}
</style>
<style>
.card svg {
  width: 80px;
  height: 80px;
  display: block;
}
</style>
