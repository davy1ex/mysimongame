<template>
    <div id="app">
      <div class="game-board">
        <game-button 
          v-for="(color, index) in colors"
          :key="index"
          :color="{ ...color, index }"
          @click="buttonClicked(index)"
          :index="index"
        />
      </div>
      
      <button @click="startGame">Начать игру</button>
    </div>
  </template>


<script>
import GameButton from './GameButton.vue';

export default {
name: 'SimonGame',
components: {
  GameButton
},

methods: {
startGame() {
  this.sequence = [];
  this.gameOver = false;
  this.addStep();
  this.showSequenceToPlayer();
},

showSequenceToPlayer() {
  if (this.sequence.length > 0) {
    const index = this.sequence[0];
    this.simulateButtonPress(index);
  }
},

simulateButtonPress(index) {
  this.colors[index].isLit = true;
  this.buttonClicked(index);

  setTimeout(() => {
    this.colors[index].isLit = false;
  }, 1500);
},


addStep() {
    this.sequence.push(Math.floor(Math.random() * 4));
    this.showSequence = true;
    this.step = 0;
    
    setTimeout(() => {
        this.showSequence = false;
    }, 
    this.sequence.length * 600);
},

buttonClicked(index) {
  const colorName = this.colors[index].name;
  this.colors[index].isLit = true;

  setTimeout(() => {
    this.colors[index].isLit = false;
  }, 300);

  switch (colorName) {
    case 'red':
      this.playTone(440); // A4
      break;
    case 'green':
      this.playTone(494); // B4
      break;
    case 'blue':
      this.playTone(523); // C5
      break;
    case 'yellow':
      this.playTone(587); // D5
      break;
  }

    console.log(index)
},

playTone(freq) {
  const oscillator = this.audioCtx.createOscillator();
  const gainNode = this.audioCtx.createGain();

  oscillator.connect(gainNode);
  gainNode.connect(this.audioCtx.destination);

  oscillator.type = 'sine'; 
  oscillator.frequency.value = freq;
  gainNode.gain.value = 0.1;

  oscillator.start();

  setTimeout(() => {
    oscillator.stop();
  }, 300);
},},

data() {
  return {
    audioCtx: new (window.AudioContext || window.webkitAudioContext)(),

    colors: [
      { name: 'red', isLit: false },
      { name: 'green', isLit: false },
      { name: 'blue', isLit: false },
      { name: 'yellow', isLit: false }
    ],
    sequence: [],
    step: 0,
    showSequence: false,
    gameOver: false,
  };
},}

</script>