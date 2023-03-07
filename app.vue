<template>
  <div class="home">
    <span class="text">
      Le cours actuel du bufalo est de:
    </span>
    <span class="value">
      {{ currentValue }} gorgées
    </span>
  </div>
</template>

<script setup lang="ts">
const maxValue = 10;
const minValue = 1;

const currentValue = Math.floor(Math.random() * maxValue) + minValue;
</script>

<style>
html {
  background-color: #2C3333;
  color: #CBE4DE;
  font-family: 'Roboto', sans-serif;
}

body {
  margin: 0;
}

.home {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
}

.text {
  font-size: 1.5rem;
}

.value {
  font-size: 2rem;
  font-weight: bold;
}
</style>