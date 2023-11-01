<script setup>
import { ref, onMounted } from "vue";
import Square from "./components/Square.vue";
import ConfettiExplosion from "vue-confetti-explosion"; // Credits: https://github.com/valgeirb/vue-confetti-explosion/tree/main

let dMode = ref("dark-mode");
let dBorder = ref("light-border");
let isDarkMode = ref(true);
let player = ref("X");
let winnerMsg = ref("");
let gameIsOver = ref(false);
const board = ref(null);
const squares = ref(["", "", "", "", "", "", "", "", ""]);

onMounted(() => {});

const onClick = (i) => {
  if (squares.value[i] !== "") return;
  squares.value[i] = player.value;
  let checking = checkScore(); // check if game is over
  if (squares.value.every((el) => el !== "") && !checking) {
    gameIsOver = true;
    winnerMsg.value = "Draw!";
  }
  player.value = player.value == "X" ? "O" : "X";
};

const checkScore = () => {
  const winConditions = [
    [0, 1, 2],
    [0, 3, 6],
    [3, 4, 5],
    [1, 4, 7],
    [6, 7, 8],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  let res = false;

  winConditions.forEach((a) => {
    let cross = a.every((n) => squares.value[n] == "X"); // check if all squares on a single win condition --
    let circle = a.every((n) => squares.value[n] == "O"); // (1 array of winConditions) are X's or O's //
    if (cross) {
      gameOver(0, a);
      res = true;
    } 
    else if (circle) {
      gameOver(1, a);
      res = true;
    }
  });
  return res;
};

const gameOver = (p, arr) => {
  arr.forEach(e => {
    board.value.children[e].classList.add('highlight');
  });
  gameIsOver = true;
  winnerMsg.value = p == 0 ? "Player X has won!" : "Player O has won!"; // 0 is X, else is O
  clickable(false);
};

const clickable = (val) => {
  board.value.style.pointerEvents = val ? "auto" : "none"; // make the board clickable or not
};

const reset = () => { 
  squares.value = ["", "", "", "", "", "", "", "", ""];
  for (var i = 0; i < board.value.childElementCount; i++) {
    board.value.children[i].classList.remove('highlight');
  }
  clickable(true);
  player.value = "X";
  winnerMsg.value = "";
  gameIsOver = false;
};

const setDarkMode = () => {
  isDarkMode = !isDarkMode;
  dMode.value = isDarkMode ? "dark-mode" : "light-mode";
  dBorder.value = isDarkMode ? "light-border" : "dark-border";
};
</script>

<template>
  <div class="main-container" :class="dMode">
    <!-- Navbar start -->
    <nav>
      <div class="form-check form-switch dm-container">
        <span>
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="16"
            height="16"
            fill="currentColor"
            class="bi bi-moon"
            viewBox="0 0 16 16">
            <path
              d="M6 .278a.768.768 0 0 1 .08.858 7.208 7.208 0 0 0-.878 3.46c0 4.021 3.278 7.277 7.318 7.277.527 0 1.04-.055 1.533-.16a.787.787 0 0 1 .81.316.733.733 0 0 1-.031.893A8.349 8.349 0 0 1 8.344 16C3.734 16 0 12.286 0 7.71 0 4.266 2.114 1.312 5.124.06A.752.752 0 0 1 6 .278zM4.858 1.311A7.269 7.269 0 0 0 1.025 7.71c0 4.02 3.279 7.276 7.319 7.276a7.316 7.316 0 0 0 5.205-2.162c-.337.042-.68.063-1.029.063-4.61 0-8.343-3.714-8.343-8.29 0-1.167.242-2.278.681-3.286z" />
          </svg>
        </span>
        <span>
          <input style="width: 32px" class="form-check-input" type="checkbox" role="switch" @click="setDarkMode()" />
        </span>
        <span>
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="16"
            height="16"
            fill="#12181B"
            class="bi bi-brightness-high"
            viewBox="0 0 16 16">
            <path
              d="M8 11a3 3 0 1 1 0-6 3 3 0 0 1 0 6zm0 1a4 4 0 1 0 0-8 4 4 0 0 0 0 8zM8 0a.5.5 0 0 1 .5.5v2a.5.5 0 0 1-1 0v-2A.5.5 0 0 1 8 0zm0 13a.5.5 0 0 1 .5.5v2a.5.5 0 0 1-1 0v-2A.5.5 0 0 1 8 13zm8-5a.5.5 0 0 1-.5.5h-2a.5.5 0 0 1 0-1h2a.5.5 0 0 1 .5.5zM3 8a.5.5 0 0 1-.5.5h-2a.5.5 0 0 1 0-1h2A.5.5 0 0 1 3 8zm10.657-5.657a.5.5 0 0 1 0 .707l-1.414 1.415a.5.5 0 1 1-.707-.708l1.414-1.414a.5.5 0 0 1 .707 0zm-9.193 9.193a.5.5 0 0 1 0 .707L3.05 13.657a.5.5 0 0 1-.707-.707l1.414-1.414a.5.5 0 0 1 .707 0zm9.193 2.121a.5.5 0 0 1-.707 0l-1.414-1.414a.5.5 0 0 1 .707-.707l1.414 1.414a.5.5 0 0 1 0 .707zM4.464 4.465a.5.5 0 0 1-.707 0L2.343 3.05a.5.5 0 1 1 .707-.707l1.414 1.414a.5.5 0 0 1 0 .708z" />
          </svg>
        </span>
      </div>
    </nav>
    <!-- Navbar end -->

    <div class="container">
      <p class="text title display-2" :class="dMode">TIC TAC TOE</p>
      <p v-show="!gameIsOver" class="text subtitle display-6" :class="dMode">
        Player
        <span :class="{ 'text-success': player == 'X', 'text-danger': player == 'O' }"> {{ player }} </span>'s turn!
      </p>
      <p
        v-show="gameIsOver"
        class="text text-center display-6"
        :class="{ 'text-success': player == 'O', 'text-danger': player == 'X' }">
        {{ winnerMsg }}
      </p>
      <ConfettiExplosion v-if="gameIsOver && winnerMsg != 'Draw!'"/>
      <div ref="board" class="board">
        <Square
          :class="dBorder"
          :key="i"
          v-for="i in 9"
          @click="onClick(i - 1)"
          :innerText="squares[i - 1]" />
      </div>
      <button type="button" class="btn btn-danger" @click="reset()">Reset</button>
    </div>
  </div>
</template>

<style scoped>
nav {
  display: flex;
  justify-content: end;
  height: 3rem;
}
.dm-container {
  width: 150px;
  display: flex;
  justify-content: space-around;
  margin: 15px 30px;
}
.dm-container > span {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 33.33%;
}
.dm-container > span > input {
  position: relative;
  left: 20px;
  bottom: 2px;
}
.light-mode {
  background-color: white;
  color: #12181b;
}
.dark-mode {
  background-color: #12181b;
  color: white;
}
.light-border {
  border: 1px solid white;
}
.dark-border {
  border: 1px solid #12181b;
}
.highlight {
  background-color: #b6ba3d;
}
.main-container {
  width: 100vw;
  height: 100vh;
}
.container {
  padding-top: 1rem;
  display: flex;
  justify-content: space-around;
  align-items: center;
  flex-direction: column;
  flex-wrap: wrap;
  width: 100%;
}
a {
  text-decoration: none;
}
.white {
  color: white !important;
}
.board {
  margin: 1rem;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  width: 456px;
  height: 451px;
}
</style>
