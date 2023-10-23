<script setup>
import { ref } from "vue";
import { confetti } from "./confetti";

let isDarkMode = ref(true);
let player = ref("X");
let gameIsOver = ref(false);
let winnerMsg = ref("");
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
let alreadyClicked = [-1, -1, -1, -1, -1, -1, -1, -1, -1];

const onClick = (i) => {
  if (alreadyClicked.includes(i)) return;

  const elem = document.getElementById(i);
  elem.innerHTML =
    player.value == "X"
      ? '<div class="cross text-success fs-1">X</div>'
      : '<div class="circle text-danger fs-1">O</div>';

  let checking = checkScore();
  alreadyClicked[i - 1] = i;
  if (!alreadyClicked.includes(-1) && !checking) {
    gameIsOver = true;
    winnerMsg.value = "Draw!";
  }
  player.value = player.value == "X" ? "O" : "X";
};

const checkScore = () => {
  let res = false;
  const elems = document.querySelectorAll(".square");

  winConditions.forEach((a) => {
    let cross = a.every((n) =>
      elems[n].firstChild?.classList.contains("cross")
    );
    if (cross) {
      gameIsOver = true;
      winnerMsg.value = "Player X has won!";
      isItClickable(false);
      confetti();
    }
  });
  winConditions.forEach((a) => {
    let circle = a.every((n) =>
      elems[n].firstChild?.classList.contains("circle")
    );
    if (circle) {
      gameIsOver = true;
      winnerMsg.value = "Player O has won!";
      isItClickable(false);
      res = true;
      confetti();
    }
  });
  return res;
};

const isItClickable = (val) => {
  const e = document.querySelectorAll(".square");
  e.forEach((s) => {
    s.style.pointerEvents = !val ? "none" : "auto";
  });
};

const reset = () => {
  document.getElementById("confetti")?.remove();
  const elem = document.querySelectorAll("a");
  elem.forEach((e) => {
    e.innerHTML = "";
  });
  isItClickable(true);
  alreadyClicked = [-1, -1, -1, -1, -1, -1, -1, -1, -1];
  player.value = "X";
  winnerMsg.value = "";
  gameIsOver = false;
};

const setDarkMode = () => {
  const body = document.querySelector("body");
  const text = document.querySelectorAll(".text");
  const squares = document.querySelectorAll(".square");
  isDarkMode = !isDarkMode;

  if (!isDarkMode) {
    body.style.backgroundColor = "white";
    text.forEach((x) => {
      x.style.color = "#12181B";
    });
    squares.forEach((x) => {
      x.style.borderColor = "#12181B";
    });
  } else {
    body.style.backgroundColor = "#12181B";
    text.forEach((x) => {
      x.style.color = "white";
    });
    squares.forEach((x) => {
      x.style.borderColor = "white";
    });
  }
};
</script>

<template>
  <nav>
    <div class="form-check form-switch dark-mode">
      <span>
        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-moon"
          viewBox="0 0 16 16">
          <path
            d="M6 .278a.768.768 0 0 1 .08.858 7.208 7.208 0 0 0-.878 3.46c0 4.021 3.278 7.277 7.318 7.277.527 0 1.04-.055 1.533-.16a.787.787 0 0 1 .81.316.733.733 0 0 1-.031.893A8.349 8.349 0 0 1 8.344 16C3.734 16 0 12.286 0 7.71 0 4.266 2.114 1.312 5.124.06A.752.752 0 0 1 6 .278zM4.858 1.311A7.269 7.269 0 0 0 1.025 7.71c0 4.02 3.279 7.276 7.319 7.276a7.316 7.316 0 0 0 5.205-2.162c-.337.042-.68.063-1.029.063-4.61 0-8.343-3.714-8.343-8.29 0-1.167.242-2.278.681-3.286z" />
        </svg>
      </span>
      <span>
        <input style="width: 32px;" class="form-check-input" type="checkbox" role="switch" @click="setDarkMode()" />
      </span>
      <span>
        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="#12181B" class="bi bi-brightness-high"
          viewBox="0 0 16 16">
          <path
            d="M8 11a3 3 0 1 1 0-6 3 3 0 0 1 0 6zm0 1a4 4 0 1 0 0-8 4 4 0 0 0 0 8zM8 0a.5.5 0 0 1 .5.5v2a.5.5 0 0 1-1 0v-2A.5.5 0 0 1 8 0zm0 13a.5.5 0 0 1 .5.5v2a.5.5 0 0 1-1 0v-2A.5.5 0 0 1 8 13zm8-5a.5.5 0 0 1-.5.5h-2a.5.5 0 0 1 0-1h2a.5.5 0 0 1 .5.5zM3 8a.5.5 0 0 1-.5.5h-2a.5.5 0 0 1 0-1h2A.5.5 0 0 1 3 8zm10.657-5.657a.5.5 0 0 1 0 .707l-1.414 1.415a.5.5 0 1 1-.707-.708l1.414-1.414a.5.5 0 0 1 .707 0zm-9.193 9.193a.5.5 0 0 1 0 .707L3.05 13.657a.5.5 0 0 1-.707-.707l1.414-1.414a.5.5 0 0 1 .707 0zm9.193 2.121a.5.5 0 0 1-.707 0l-1.414-1.414a.5.5 0 0 1 .707-.707l1.414 1.414a.5.5 0 0 1 0 .707zM4.464 4.465a.5.5 0 0 1-.707 0L2.343 3.05a.5.5 0 1 1 .707-.707l1.414 1.414a.5.5 0 0 1 0 .708z" />
        </svg>
      </span>
    </div>
  </nav>
  <div class="container">
    <p class="text title display-1">TIC TAC TOE</p>
    <p v-show="!gameIsOver" class="text subtitle display-5">
      Player
      <span :class="{ 'text-success': player == 'X', 'text-danger': player == 'O', 'white': winnerMsg == 'Draw!' }">
        {{player}}
      </span>'s turn!
    </p>
    <p v-show="gameIsOver" class="text text-center display-6"
      :class="{ 'text-success': player == 'O', 'text-danger': player == 'X' }">
      {{ winnerMsg }}
    </p>
    <div id="board" class="board">
      <a :id="i" @click="onClick(i)" class="square" v-for="i in 9"></a>
    </div>
    <button type="button" class="btn btn-danger" @click="reset()">Reset</button>
  </div>
</template>

<style scoped>
nav {
  display: flex;
  justify-content: end;
  height: 3rem;
}

.dark-mode {
  width: 150px;
  display: flex;
  justify-content: space-around;
  margin: 15px 30px;
}

.dark-mode>span {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 33.33%;
}

.dark-mode>span>input {
  position: relative;
  left: 20px;
  bottom: 2px;
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

.board {
  margin: 1rem;
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  width: 450px;
  height: 450px;
}

.square {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 150px;
  height: 150px;
  border: 1px solid white;
}
</style>
