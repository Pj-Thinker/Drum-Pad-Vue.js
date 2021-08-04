<template>
  <main class="container">
    <section class="pads-container">
      <div
        class="drum-pad"
        v-for="pad in padsBank"
        :key="pad.id"
        @click="playSound(pad)"
        :id="pad.id"
      >
        <audio :src="pad.url" class="clip" :id="pad.keyTrigger" />
        {{ pad.keyTrigger }}
      </div>
    </section>
    <section class="controller">
      <div class="power">
        <input @click="power = !power" type="checkbox" id="toggle" checked />
      </div>
      <div class="set">
        <input @click="handleBank" type="checkbox" class="setBtn" checked />
      </div>
      <p id="display">{{ display }}</p>
      <div class="volume">
        <span>Volume</span>
        <input
          type="range"
          step="0.01"
          min="0"
          max="1"
          @input="handleSlider"
          v-model="currentValue"
          class="volume-slider"
        />
      </div>
    </section>
    <a href="https://www.instagram.com/the.pendar/" target="_blank"
      >Pendar | IG ❤</a
    >
  </main>
</template>

<script>
import { ref } from '@vue/reactivity';
import { bankOne, bankTwo } from './assets/sounds';
import { onMounted } from '@vue/runtime-core';

export default {
  name: 'App',

  setup() {
    const padsBank = ref([]);
    const power = ref(true);
    const soundSet = ref(true);
    const currentValue = ref(0.3);
    const display = ref('');

    bankOne.forEach(item => padsBank.value.push(item));

    const handleBank = () => {
      padsBank.value = [];
      soundSet.value = !soundSet.value;

      if (soundSet.value) {
        bankOne.forEach(item => padsBank.value.push(item));
        updateDisplay('Heater Kit');
      } else {
        bankTwo.forEach(item => padsBank.value.push(item));
        updateDisplay('Smooth Piano Kit');
      }
    };

    const playSound = pad => {
      if (power.value) {
        const sound = document.getElementById(pad.keyTrigger);
        sound.currentTime = 0;
        sound.play();
        updateDisplay(pad.id);
      }
    };

    const handleSlider = () => {
      const audios = document.querySelectorAll('audio');
      audios.forEach(sound => (sound.volume = currentValue.value));
      updateDisplay(`Volume: ${currentValue.value}`);
    };

    document.addEventListener('keydown', e => {
      padsBank.value.forEach(pad => {
        if (e.key.toLowerCase() === pad.keyTrigger.toLowerCase()) {
          playSound(pad);
        }
      });

      animation(e.key.toUpperCase());
    });

    document.addEventListener('click', e => {
      animation(e.target.innerText);
    });

    const animation = e => {
      const pads = document.querySelectorAll('.drum-pad');
      pads.forEach(pad => {
        if (pad.innerText == e) {
          pad.classList.add('clicked');
        }
        setTimeout(() => {
          pad.classList.remove('clicked');
        }, 100);
      });
    };

    const updateDisplay = str => {
      display.value = str;
    };

    return {
      padsBank,
      playSound,
      power,
      handleBank,
      soundSet,
      currentValue,
      handleSlider,
      display,
    };
  },
};
</script>

<style>
:root {
  --body: #dfe3ec;
  --text: rgb(20, 20, 20);
}

body {
  background: var(--body);
  box-sizing: border-box;
  font-family: 'Helvetica', 'Arial', sans-serif;
}

.container {
  margin: 5rem auto;
  width: 400px;
  height: 580px;
  padding: 2rem 3rem;
  border-radius: 1rem;
  box-shadow: 2.7px 2.7px 5px #898c91, -2.7px -2.7px 5px #ffffff;

  display: flex;
  flex-direction: column;
  justify-content: space-around;
  align-items: center;
}

.pads-container {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-template-rows: auto;
  gap: 1.125rem;
}

.controller {
  display: grid;
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 1fr 1fr 1fr;
}

.drum-pad {
  width: 100px;
  height: 100px;
  cursor: pointer;
  font-size: 1.65rem;
  font-weight: bold;
  user-select: none;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(145deg, #d6dce9, #dfe3ec);
  border-radius: 0.4rem;
  box-shadow: 0.6px 0.6px 1px #a5a7aa, -0.6px -0.6px 1px #ffffff;
}

.power {
  grid-column: 1/2;
  grid-row: 1/2;
  align-self: center;
}

.set {
  grid-column: 1/2;
  grid-row: 2/3;
  align-self: center;
}

.clicked {
  background: #eef0f4;
  box-shadow: inset 9.91px 9.91px 15px #d9dade,
    inset -9.91px -9.91px 15px #ffffff;
}

.volume {
  grid-column: 1/3;
  grid-row: 3/4;
  font-size: 1.125rem;
  font-weight: bold;
  color: #0071e3;
  margin-top: 1rem;
  align-self: center;
}

.volume span {
  margin-left: 0.5rem;
}

.volume-slider {
  width: 50%;
  height: 10px;
  margin-left: 3.1rem;
}

.volume-slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  border-radius: 100%;
  background-color: #0071e3;
  height: 18px;
  width: 18px;
}

#display {
  grid-column: 2/3;
  grid-row: 1/3;
  width: 160px;
  height: 80px;

  text-align: center;
  line-height: 80px;
  color: coral;
  font-weight: bold;

  background: var(--body);
  border-radius: 0.5rem;
  box-shadow: inset 2.7px 2.7px 10px #c1c2c6, inset -2.7px -2.7px 10px #ffffff;
}

.power input[type='checkbox'] {
  -webkit-appearance: none;
  outline: none;
  position: relative;
  height: 2.25rem;
  width: 4.25rem;
  border: 3px solid var(--body);
  border-radius: 1.25rem;
  cursor: pointer;
  box-shadow: 2.1px 2.1px 9px #999ba2, -2.1px -2.1px 9px #ffffff;
}

.power input[type='checkbox']::before {
  content: '';
  height: 1.5rem;
  width: 1.5rem;
  background-color: var(--body);
  position: absolute;
  margin: auto;
  top: 0;
  left: 0.3rem;
  bottom: 0;
  border-radius: 50%;
  box-shadow: 7px 7px 15px #c3c3c3, 9px 9px 16px rgba(189, 189, 189, 0.6);
  transition: 0.15s;
}

.power input[type='checkbox']::after {
  content: 'Off';
  position: absolute;
  font-size: 1rem;
  top: 0.4rem;
  right: 0.35rem;
  color: var(--text);
  font-weight: 600;
}

input[type='checkbox']:checked {
  background: #0071e3;
}

input[type='checkbox']:checked::before {
  left: 2.2rem;
  box-shadow: none;
}

.power input[type='checkbox']:checked::after {
  content: 'On';
  left: 0.5rem;
  color: #f5f5f7;
}

.set input[type='checkbox'] {
  -webkit-appearance: none;
  outline: none;
  position: relative;
  height: 2.25rem;
  width: 4.25rem;
  border: 3px solid var(--body);
  background: #0071e3;
  border-radius: 1.25rem;
  cursor: pointer;
  box-shadow: 2.1px 2.1px 9px #999ba2, -2.1px -2.1px 9px #ffffff;
}

.set input[type='checkbox']::before {
  content: '';
  height: 1.5rem;
  width: 1.5rem;
  background-color: var(--body);
  position: absolute;
  margin: auto;
  top: 0;
  left: 0.3rem;
  bottom: 0;
  border-radius: 50%;
  transition: 0.15s;
}

.set input[type='checkbox']::after {
  content: 'S/2';
  position: absolute;
  font-size: 1rem;
  top: 0.4rem;
  right: 0.35rem;
  color: #f5f5f7;
  font-weight: 600;
}

input[type='checkbox']:checked::before {
  left: 2.2rem;
  box-shadow: none;
}
.set input[type='checkbox']:checked::after {
  content: 'S/1';
  left: 0.5rem;
  color: #f5f5f7;
}

a {
  color: coral;
  text-decoration: none;
  margin-top: 1rem;
  margin-bottom: -1rem;
  font-size: 0.85rem;
}
</style>
