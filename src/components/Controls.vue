<template>
  <div class="wrapper-controls">
    <button @click="$emit('handleNewGame')" class="control btn-new">New game</button>
    <button @click="$emit('handleRollDice')" class="control btn-roll">Roll dice</button>
    <button @click="$emit('handleHoldScore')" class="control btn-hold">Hold</button>

    <input
      @input="$emit('handleChangeFinalScore', $event)"
      :value="finalScore"
      placeholder="Final score"
      class="final-score"
      :disabled="isPlaying"
    />
  </div>
</template>

<script setup>
defineProps({
  finalScore: [Number, String],
  isPlaying: Boolean
});
</script>

<style scoped>
.control {
  position: absolute;
  width: 200px;
  left: 50%;
  transform: translateX(-50%);
  color: #555;
  background: none;
  border: none;
  font-family: Lato;
  font-size: 20px;
  text-transform: uppercase;
  cursor: pointer;
  font-weight: 300;
  transition:
    background-color 0.3s,
    color 0.3s;
}
.control.disable {
  pointer-events: none;
}

.control:hover {
  font-weight: 600;
}
.control:hover i {
  margin-right: 20px;
}

.control:focus {
  outline: none;
}

.control i {
  color: #42b983;
  display: inline-block;
  margin-right: 15px;
  font-size: 32px;
  line-height: 1;
  vertical-align: text-top;
  margin-top: -4px;
  transition: margin 0.3s;
}

.btn-new {
  top: 45px;
}
.btn-roll {
  top: 403px;
}
.btn-hold {
  top: 467px;
}

.final-score {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  top: 520px;
  color: #555;
  font-size: 18px;
  font-family: 'Lato';
  text-align: center;
  padding: 10px;
  width: 160px;
  text-transform: uppercase;
}

.final-score:focus {
  outline: none;
}
</style>
