<template>
  <section id="app"
           class="memory-game">
    <div class="memory-card"
         data-framework="aurelia">
      <img class="front-face"
           src="./assets/image/angular.svg"
           alt="Aurelia" />
      <img class="back-face"
           src="./assets/image/js-badge.svg"
           alt="JS Badge" />
    </div>
    <div class="memory-card"
         data-framework="aurelia">
      <img class="front-face"
           src="./assets/image/aurelia.svg"
           alt="Aurelia" />
      <img class="back-face"
           src="./assets/image/js-badge.svg"
           alt="JS Badge" />
    </div>

    <div class="memory-card"
         data-framework="vue">
      <img class="front-face"
           src="./assets/image/vue.svg"
           alt="Vue" />
      <img class="back-face"
           src="./assets/image/js-badge.svg"
           alt="JS Badge" />
    </div>
    <div class="memory-card"
         data-framework="vue">
      <img class="front-face"
           src="./assets/image/vue.svg"
           alt="Vue" />
      <img class="back-face"
           src="./assets/image/js-badge.svg"
           alt="JS Badge" />
    </div>

    <div class="memory-card"
         data-framework="angular">
      <img class="front-face"
           src="./assets/image/angular.svg"
           alt="Angular" />
      <img class="back-face"
           src="./assets/image/js-badge.svg"
           alt="JS Badge" />
    </div>
    <div class="memory-card"
         data-framework="angular">
      <img class="front-face"
           src="./assets/image/angular.svg"
           alt="Angular" />
      <img class="back-face"
           src="./assets/image/js-badge.svg"
           alt="JS Badge" />
    </div>

    <div class="memory-card"
         data-framework="ember">
      <img class="front-face"
           src="./assets/image/ember.svg"
           alt="Ember" />
      <img class="back-face"
           src="./assets/image/js-badge.svg"
           alt="JS Badge" />
    </div>
    <div class="memory-card"
         data-framework="ember">
      <img class="front-face"
           src="./assets/image/ember.svg"
           alt="Ember" />
      <img class="back-face"
           src="./assets/image/js-badge.svg"
           alt="JS Badge" />
    </div>

    <div class="memory-card"
         data-framework="backbone">
      <img class="front-face"
           src="./assets/image/backbone.svg"
           alt="Backbone" />
      <img class="back-face"
           src="./assets/image/js-badge.svg"
           alt="JS Badge" />
    </div>
    <div class="memory-card"
         data-framework="backbone">
      <img class="front-face"
           src="./assets/image/backbone.svg"
           alt="Backbone" />
      <img class="back-face"
           src="./assets/image/js-badge.svg"
           alt="JS Badge" />
    </div>

    <div class="memory-card"
         data-framework="react">
      <img class="front-face"
           src="./assets/image/react.svg"
           alt="React" />
      <img class="back-face"
           src="./assets/image/js-badge.svg"
           alt="JS Badge" />
    </div>
    <div class="memory-card"
         data-framework="react">
      <img class="front-face"
           src="./assets/image/react.svg"
           alt="React" />
      <img class="back-face"
           src="./assets/image/js-badge.svg"
           alt="JS Badge" />
    </div>
  </section>
</template>

<script>
let hasFlippedCard = false;
let lockBoard = false;
let firstCard, secondCard;

function flipCard() {
  if (lockBoard) return;
  if (this === firstCard) return;

  this.classList.add("flip");

  if (!hasFlippedCard) {
    // first click
    hasFlippedCard = true;
    firstCard = this;

    return;
  }

  // second click
  secondCard = this;

  checkForMatch();
}

function checkForMatch() {
  let isMatch = firstCard.dataset.framework === secondCard.dataset.framework;

  isMatch ? disableCards() : unflipCards();
}

function disableCards() {
  firstCard.removeEventListener("click", flipCard);
  secondCard.removeEventListener("click", flipCard);

  resetBoard();
}

function unflipCards() {
  lockBoard = true;

  setTimeout(() => {
    firstCard.classList.remove("flip");
    secondCard.classList.remove("flip");

    resetBoard();
  }, 1500);
}

function resetBoard() {
  [hasFlippedCard, lockBoard] = [false, false];
  [firstCard, secondCard] = [null, null];
}

export default {
  name: "app",
  data() {
    return {
      cards: [],
      hasFlippedCard: false,
      lockBoard: false,
      firstCard: null,
      secondCard: null
    };
  },
  created() {
    this.$nextTick(() => {
      this.cards = document.querySelectorAll(".memory-card");
      this.cards.forEach(card => card.addEventListener("click", flipCard));
      this.shuffle();
    });
  },
  methods: {
    shuffle() {
      this.cards.forEach(card => {
        let randomPos = Math.floor(Math.random() * 12);
        card.style.order = randomPos;
      });
    }
  }
};
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

body {
  height: 100vh;
  display: flex;
  background: #060ab2;
}

.memory-game {
  width: 640px;
  height: 640px;
  margin: auto;
  display: flex;
  flex-wrap: wrap;
  perspective: 1000px;
}

.memory-card {
  width: calc(25% - 10px);
  height: calc(33.333% - 10px);
  margin: 5px;
  position: relative;
  transform: scale(1);
  transform-style: preserve-3d;
  transition: transform 0.5s;
  box-shadow: 1px 1px 1px rgba(0, 0, 0, 0.3);
}

.memory-card:active {
  transform: scale(0.97);
  transition: transform 0.2s;
}

.memory-card.flip {
  transform: rotateY(180deg);
}

.front-face,
.back-face {
  width: 100%;
  height: 100%;
  padding: 20px;
  position: absolute;
  border-radius: 5px;
  background: #1c7ccc;
  backface-visibility: hidden;
}

.front-face {
  transform: rotateY(180deg);
}
</style>
