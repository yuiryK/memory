<script setup>
defineProps({
  svg: Object
})
</script>

<template>
  <div class="icon" v-html="svg.svg"></div>
</template>

<style scoped>
.icon {
  display: grid;
  place-items: center;
  width: 100px;
  height: 100px;
  padding: 10px;
  border: 2px solid #ccc;
  border-radius: 12px;
  background-color: #f8f8f8;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.05);
  transition: box-shadow 0.3s ease;
}

.icon:hover {
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

</style>
<style>
.icon svg {
  width: 80px;
  height: 80px;
  display: block;
}
</style>
