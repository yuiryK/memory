<script setup>

</script>

<template>
  <div class="icon">
      <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-bell-fill" viewBox="0 0 16 16">
           <path d="M8 16a2 2 0 0 0 2-2H6a2 2 0 0 0 2 2m.995-14.901a1 1 0 1 0-1.99 0A5 5 0 0 0 3 6c0 1.098-.5 6-2 7h14c-1.5-1-2-5.902-2-7 0-2.42-1.72-4.44-4.005-4.901"/>
      </svg>
  </div>
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

.svg-wrapper:hover {
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.icon svg {
  width: 80px;
  height: 80px;
  display: block;
}
</style>
