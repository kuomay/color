<template>
  <div class="slider">
    <h1 :style="{ backgroundColor: rgbaCode }">RGB to HEX</h1>
    <div>
      <span class="red">R</span>
      <input type="range" v-model.number="rgb[0]" min="0" max="255">
      <span class="red">{{ rgb[0] }}</span>
    </div>

    <div>
      <span class="green">G</span>
      <input type="range" v-model.number="rgb[1]" min="0" max="255">
      <span class="green">{{ rgb[1] }}</span>
    </div>

    <div>
      <span class="blue">B</span>
      <input type="range" v-model.number="rgb[2]" min="0" max="255">
      <span class="blue">{{ rgb[2] }}</span>
    </div>

    <div>
      <span class="alpha">A</span>
      <input type="range" v-model.number="alpha" min="0" max="1" step="0.01">
      <span class="alpha">{{ alpha.toFixed(2) }}</span>
    </div>

    <div class="color-input">
      <label for="hexInput" class="label">Hex Code:</label>
      <input type="text" id="hexInput" v-model.trim="userHexCode" @input="updateColorFromHex">
    </div>

    <div class="flex-container">
      <div class="flex-item item-1" @click="copyHexCode">
        <h1 class="hexCode">{{ hexCode }}</h1>
      </div>
      <div class="flex-item item-2"><h1 class="rgbaCode">{{ rgbaCode }}</h1></div>
    </div>
  </div>
</template>

<script setup>
import { ref, watchEffect } from 'vue';

const rgb = ref([0, 0, 0]);
const alpha = ref(1);
const hexCode = ref('#FFFFFF');
const rgbaCode = ref('rgba(0, 0, 0, 1.00)');
const userHexCode = ref('#FFFFFF');  

const copyHexCode = () => {
  const el = document.createElement('textarea');
  el.value = hexCode.value;
  document.body.appendChild(el);
  el.select();
  document.execCommand('copy');
  document.body.removeChild(el);
  alert('已複製');
};

function updateColorFromHex() {
  const hex = userHexCode.value.replace(/^#/, '');
  const bigint = parseInt(hex, 16);
  const r = (bigint >> 16) & 255;
  const g = (bigint >> 8) & 255;
  const b = bigint & 255;
  rgb.value = [r, g, b];
}

watchEffect(() => {
  hexCode.value = `#${rgb.value.map(c => toHex(c)).join('')}`;
  rgbaCode.value = `rgba(${rgb.value.join(', ')}, ${alpha.value.toFixed(2)})`;
});

function toHex(c) {
  const hex = c.toString(16);
  return hex.length === 1 ? '0' + hex : hex;
}

updateColorFromHex();
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

.hexCode {
  color: white;
  font-size: 24px;
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
  width: 60%;
}

.item-2 {
  width: 60%;
}

.color-input {
  margin-top: 10px;
  margin-bottom: 10px;
}

.color-input label {
  font-weight: bold;
  color: black;
}

.color-input input {
  width: 100px; 
  padding: 5px;
  border: 1px solid #ccc;
  border-radius: 3px;
  margin-left: 10px; 
}

</style>
