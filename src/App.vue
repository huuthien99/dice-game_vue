<script setup>
import { computed, reactive } from 'vue';
import Dices from './components/Dices.vue';
import Players from './components/Players.vue';
import Controls from './components/Controls.vue';

const data = reactive({
  isPlaying: false,
  isOpenPopup: false,
  activePlayer: 0,
  scoresPlayer: [0, 0],
  dices: [1, 1],
  currentScore: 0,
  finalScore: 100
});

const handleChangeFinalScore = (e) => {
  data.finalScore = +e.target.value;
};

const isWinner = computed(() => {
  const { scoresPlayer, finalScore } = data;
  if (scoresPlayer[0] >= finalScore || scoresPlayer[1] >= finalScore) {
    data.isPlaying = false;
    return true;
  }
  return false;
});

const handleNewGame = () => {
  data.isPlaying = true;
  data.activePlayer = 0;
  data.scoresPlayer = [0, 0];
  data.dices = [1, 1];
  data.currentScore = 0;
};

const nextPlayer = () => {
  data.activePlayer = data.activePlayer == 0 ? 1 : 0;
  data.currentScore = 0;
};

const handleRollDice = () => {
  if (!data.isPlaying) {
    alert('Vui lòng bắt đầu game mới!');
    return;
  }
  const diceOne = Math.floor(Math.random() * 6) + 1;
  const diceTwo = Math.floor(Math.random() * 6) + 1;

  data.dices.splice(0, 2, diceOne, diceTwo);

  if (diceOne == 1 || diceTwo == 1) {
    setTimeout(() => {
      alert('Bạn đã giao vào ô 1!');
      nextPlayer();
    }, 100);
  } else {
    data.currentScore += diceOne + diceTwo;
  }
};

const handleHoldScore = () => {
  if (!data.isPlaying) {
    alert('Vui lòng bắt đầu game mới!');
    return;
  }
  const newScore = data.scoresPlayer[data.activePlayer] + data.currentScore;
  data.scoresPlayer.splice(data.activePlayer, 1, newScore);
  if (isWinner.value) return;
  nextPlayer();
};
</script>

<template>
  <div class="wrapper clearfix">
    <Players
      :currentScore="data.currentScore"
      :scoresPlayer="data.scoresPlayer"
      :activePlayer="data.activePlayer"
      :isWinner="isWinner"
    />
    <Controls
      :isPlaying="data.isPlaying"
      :finalScore="data.finalScore"
      @handleChangeFinalScore="handleChangeFinalScore"
      @handleNewGame="handleNewGame"
      @handleRollDice="handleRollDice"
      @handleHoldScore="handleHoldScore"
    />
    <Dices :dices="data.dices" />
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.clearfix::after {
  content: '';
  display: table;
  clear: both;
}

body {
  background-image: linear-gradient(rgba(62, 20, 20, 0.4), rgba(62, 20, 20, 0.4)),
    url('/assets/back.jpg');
  background-size: cover;
  background-position: center;
  font-weight: 300;
  position: relative;
  height: 100vh;
  color: #555;
}

.wrapper {
  width: 1000px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
  overflow: hidden;
  border-radius: 3px;
}
</style>
