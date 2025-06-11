<script setup>
defineProps({
  svg: Object,
  flipped: Boolean,
  matched: Boolean,
});
defineEmits(['click']);
</script>

<template>
  <div class="card" @click="$emit('click')" :class="{ hidden: matched }">
    <div class="card-inner" :class="{ flipped: flipped || matched }">
      <!-- Front side (question mark) -->
      <div class="card-face card-front">❓</div>
      <!-- Back side (icon) -->
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

.card-front {
  width: 80px;
  height: 80px;
}

.card-back {
  transform: rotateY(180deg);
  font-size: 32px;
}
.card.hidden {
  opacity: 0;
  pointer-events: none;
  visibility: hidden; /* Можно скрыть для скринридеров и т.п. */
  transition: opacity 0.3s ease;
}
</style>

<style>
.card svg {
  width: 80px;
  height: 80px;
  display: block;
}
</style>
