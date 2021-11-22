<template>
  <div class="game">
    <header>
      <div class="restart" @click="restart">Restart</div>
      <h1 class="logo">The Office</h1>
      <div class="scores">
        <h5>Turns / Time</h5>
        <h2>{{ turns }} / {{ time.toString().padStart(2, 0) }}</h2>
      </div>
    </header>
    <div class="cards">
      <cards
        v-for="(card, index) in cards"
        :key="index"
        :cardId="card.cardId"
        :isFliped="card.isFliped"
        :isMatched="card.isMatched"
        @click="selectCard(index, card.cardId)"
      >
      </cards>
    </div>
    <div v-show="finishGame" class="results">
      <h1>You Win!!!</h1>
      <h2>Turns: {{ turns }}</h2>
      <h2>Time: {{ time }}</h2>
      <h3>Introduce Your Name:</h3>
      <input
        @keyup.enter="saveScore"
        type="text"
        maxlength="10"
        v-model="playername"
      />
      <button
        class="saveScore"
        @click="saveScore"
        :disabled="!playername.length"
      >
        Save Score
      </button>
      <div class="retry" @click="restart">Retry</div>
    </div>
  </div>
</template>

<script>
import Localbase from "localbase";
let db = new Localbase("db");
import cards from "../ui/Cards.vue";
export default {
  name: "Game",
  components: {
    cards,
  },
  data() {
    return {
      finishGame: false,
      playername: "",
      cards: [],
      flippedCards: [],
      turns: 0,
      time: 0,
      matcheds: 0,
    };
  },
  methods: {
    selectTen() {
      const results = [];
      const numbers = [];
      const limit = 10;
      while (results.length < limit) {
        // *18 es porque son 17 personajes
        const number = Math.floor(Math.random() * 18);
        if (!numbers.includes(number) && number !== 0) {
          numbers.push(number);
          results.push({ cardId: number, isFliped: false, isMatched: false });
        }
      }
      const clonedResults = JSON.parse(JSON.stringify(results));
      this.cards = results
        .concat(clonedResults)
        .sort(() => Math.random() - 0.5);
    },
    selectCard(idx, cid) {
      if (this.flippedCards.length < 2) {
        this.cards[idx].isFliped = true;
        this.flippedCards.push({ index: idx, cardId: cid });
      }
      if (this.flippedCards.length === 2) {
        this.turns++;
        if (this.flippedCards[0].cardId === this.flippedCards[1].cardId) {
          setTimeout(() => {
            this.cards[this.flippedCards[0].index].isMatched = true;
            this.cards[this.flippedCards[1].index].isMatched = true;
            this.flippedCards = [];
            this.matcheds++;
          }, 250);
        } else {
          setTimeout(() => {
            this.cards[this.flippedCards[0].index].isFliped = false;
            this.cards[this.flippedCards[1].index].isFliped = false;
            this.flippedCards = [];
          }, 500);
        }
      }
    },
    startCounter() {
      setInterval(() => {
        if (this.finishGame === false) {
          this.time++;
        }
      }, 1000);
    },
    restart() {
      this.matcheds = 0;
      this.playername = "";
      this.time = 0;
      this.turns = 0;
      this.cards.forEach((card) => {
        card.isFliped = false;
        card.isMatched = false;
      });
      setTimeout(() => {
        this.selectTen();
      }, 500);
      this.finishGame = false;
    },
    saveScore() {
      let score = {
        player: this.playername,
        turns: this.turns,
        time: this.time,
      };
      db.collection("scores").add(score);
      setTimeout(() => {
        this.$emit("changePage", "Score");
      }, 1000);
    },
  },
  watch: {
    matcheds: function (val) {
      if (val === 10) {
        this.finishGame = true;
      }
    },
    time: function (val) {
      if (val > 59) {
        if (confirm("The Time is Over!\nRestart?")) {
          this.restart();
        } else {
          this.$emit("changePage", "Menu");
        }
      }
    },
  },
  mounted() {
    this.selectTen();
    this.startCounter();
  },
};
</script>

<style lang="scss" scoped>
.game {
  position: relative;
  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1em;
    .restart {
      text-align: center;
      background-color: rgb(241, 239, 239);
      padding: 0.8rem 1.5em;
      font-size: 1rem;
      font-family: monospace;
      box-shadow: 0px 3px 5px #48494b;
      transition: all 0.3s;
      cursor: pointer;
      border-radius: 5px;
      @media (max-width: 760px) {
        font-size: small;
        padding: 0.5em 1em;
      }
      &:hover {
        box-shadow: 0px 10px 15px -5px #48494bee;
        transform: scale(1.02);
      }
      &:active {
        box-shadow: 0px 4px 8px #48494bee;
        transform: scale(0.98);
      }
    }
    .logo {
      font-size: 3em;
      font-weight: bolder;
      color: #fff;
      text-shadow: 1px 2px 3px rgba(23, 0, 0, 0.5);
      @media (max-width: 760px) {
        font-size: 1.5em;
      }
    }
    .scores {
      text-align: center;
      background-color: rgb(241, 239, 239);
      padding: 0.4rem 1.5em;
      font-size: 1rem;
      font-family: monospace;
      box-shadow: 0px 3px 5px #48494b;
      transition: all 0.3s;
      border-radius: 5px;
      @media (max-width: 780px) {
        font-size: small;
        padding: 0.2em 0.6em;
      }
      h5,
      h2 {
        font-family: monospace;
      }
    }
  }
}
.cards {
  margin-right: auto;
  margin-left: auto;
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 1em;
  @media (max-width: 480px) {
    gap: 0.5em;
    grid-template-columns: repeat(4, 1fr);
  }
}
.results {
  position: absolute;
  z-index: 10;
  top: 25%;
  left: 0;
  right: 0;
  width: 70%;
  margin: auto;
  border-radius: 15px;
  border: 5px white solid;
  background: rgba(0, 0, 0, 0.9);
  padding: 2.5em;
  @media (max-width: 780px) {
    padding: 1em;
    width: 100%;
  }
  h1 {
    text-align: center;
    font-size: 3em;
    font-weight: bolder;
    color: #fff;
    text-shadow: 1px 2px 3px rgba(23, 0, 0, 0.5);
    margin-bottom: 0.5em;
  }
  h2 {
    font-weight: bolder;
    color: #fff;
    margin: 0.5em 0;
  }
  h3 {
    color: #fff;
    text-align: center;
  }
  input {
    width: 100%;
    padding: 12px 20px;
    border: 1px solid #ccc;
    border-radius: 4px;
    margin: 0.5em auto;
    font-family: monospace;
    &:focus {
      outline: none;
    }
  }
  .saveScore {
    width: 100%;
    text-align: center;
    background-color: rgb(241, 239, 239);
    padding: 0.4rem 1.5em;
    font-size: 1rem;
    font-family: monospace;
    box-shadow: 0px 3px 5px #48494b;
    transition: all 0.3s;
    border-radius: 5px;
    cursor: pointer;
    &:hover {
      box-shadow: 0px 10px 15px -5px #48494bee;
      transform: scale(1.02);
    }
    &:active {
      box-shadow: 0px 4px 8px #48494bee;
      transform: scale(0.98);
    }
    &:disabled {
      background-color: #48494bee;
      cursor: none;
    }
  }
  .retry {
    cursor: pointer;
    color: #fff;
    text-align: center;
    margin-top: 1em;
  }
}
</style>