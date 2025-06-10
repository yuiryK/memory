<template>
  <div>
    <input v-model.number="sideLength" type="number" min="1" :max="maxSide" />
    <button @click="submit">Сгенерировать</button>
    <p v-if="error">{{ error }}</p>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const sideLength = ref(1);
const error = ref('');
const maxSide = 10;

const emit = defineEmits(['generate']);

function submit() {
  if (sideLength.value < 1 || sideLength.value > maxSide) {
    error.value = `Число должно быть от 1 до ${maxSide}`;
  } else {
    error.value = '';
    const total = sideLength.value ** 2;
    emit('generate', total);
  }
}
</script>
