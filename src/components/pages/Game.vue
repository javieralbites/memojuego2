<template>
  <h1>START</h1>
  <h2>Turns: {{ turns }}</h2>
  <h2>Time: 00:{{ time.toString().padStart(2, 0) }}</h2>
  <h2>Matcheds: {{ matcheds }}</h2>
  <div v-show="!finishGame" class="game">
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
    <input @keyup.enter="saveScore" type="text" v-model="playername" />
    <button @click="saveScore">Save Score</button>
    <button @click="restart">Restart</button>
  </div>
</template>

<script>
import Localbase from "localbase";
let db = new Localbase("db");
import cards from "../ui/Cards.vue";
export default {
  name: 'Game',
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
            // this.turns++;
          }, 250);
        } else {
          setTimeout(() => {
            this.cards[this.flippedCards[0].index].isFliped = false;
            this.cards[this.flippedCards[1].index].isFliped = false;
            this.flippedCards = [];
            // this.turns++
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
  // width: 800px;
  margin-right: auto;
  margin-left: auto;
  // background: rosybrown;
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  gap: 1em;
  @media (max-width: 480px) {
    gap: 0.5em;
    grid-template-columns: repeat(4, 1fr);
  }
}
.results {
  width: 800px;
  margin-right: auto;
  margin-left: auto;
}
</style>