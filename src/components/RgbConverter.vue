<template>
    <div class="slider">
      <h1>RGB to HEX</h1>
      <div>
        <span class="red">R</span>
        <input type="range" v-model="rgb[0]" min="0" max="255">
        <span class="red">{{ rgb[0] }}</span>
      </div>
  
      <div>
        <span class="green">G</span>
        <input type="range" v-model="rgb[1]" min="0" max="255">
        <span class="green">{{ rgb[1] }}</span>
      </div>
  
      <div>
        <span class="blue">B</span>
        <input type="range" v-model="rgb[2]" min="0" max="255">
        <span class="blue">{{ rgb[2] }}</span>
      </div>
  
      <div>
        <span class="alpha">A</span>
        <input type="range" v-model="alpha" min="0" max="1" step="0.01">
        <span class="alpha">{{ alpha.toFixed(2) }}</span>
      </div>
  
      <div class="flex-container">
        <div class="flex-item item-1"><h1 class="hexCode">{{ hexCode }}</h1></div>
        <div class="flex-item item-2"><h1 class="rgbaCode">{{ rgbaCode }}</h1></div>
      </div>
    </div>
  </template>
  
  <script setup>
  import { ref, watchEffect } from 'vue';
  
  const rgb = ref([0, 0, 0]);
  const alpha = ref(1);
  const hexCode = ref('#000000');
  const rgbaCode = ref('rgba(0, 0, 0, 1.00)');
  
  watchEffect(() => {
  if (typeof alpha.value !== 'number') {
    alpha.value = parseFloat(alpha.value);
    if (isNaN(alpha.value)) {
      console.error("alpha is not a number:", alpha.value);
      return;
    }
  }

  hexCode.value = `#${rgb.value.map(c => c.toString(16).padStart(2, '0')).join('')}`;
  const newRgbaCode = `rgba(${rgb.value.join(', ')}, ${alpha.value.toFixed(2)})`;
  document.documentElement.style.setProperty('--background-color', newRgbaCode);
  rgbaCode.value = newRgbaCode;
});

  </script>
  
  <style scoped>
  :root {
    --background-color: black;
  }
  
  body {
    font-family: "Roboto", sans-serif;
    background-color: var(--background-color);
    font-weight: bold;
    color: white;
  }
  
  .slider {
    height: 600px;
    display: flex;
    flex-direction: column;
    justify-content: space-around;
    align-items: center;
  }
  
  .red, .green, .blue, .alpha {
    border-radius: 20%;
    padding: 10px;
  }
  
  .red { background-color: red; }
  .green { background-color: green; }
  .blue { background-color: blue; }
  .alpha { background-color: grey; }
  
  h1 {
    color: #ffffff;
    background-color: rgba(210, 155, 155, 0.5);
    border: 2px solid #000000;
    border-radius: 8px;
    margin: 10px 10px;
    padding: 5px 30px;
  }
  
  .flex-container {
    width: 750px;
    height: 120px;
    margin-top: 20px;
    display: flex;
    flex-wrap: wrap;
    flex-direction: column;
    align-items: center;
  }

  .flex-item {
  display: flex;
  align-items: center;
  justify-content: center;
  height: calc(100% / 1);
}

  .item-1 {
  width: 100%;
  }

  .item-2 {
    width: 60%;
  }

  </style>
  

