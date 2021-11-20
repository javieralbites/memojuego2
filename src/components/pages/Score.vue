<template>
  <h1>The Office</h1>
  <h3>The Memory Game</h3>
  <h2>SCORE</h2>
  <div class="table">
    <table>
      <tr>
        <th>#</th>
        <th>Player</th>
        <th>Turns</th>
        <th>Time</th>
      </tr>
      <tr v-for="(score, index) in sorted" :key="index">
        <td>{{ index + 1 }}</td>
        <td>{{ score.player }}</td>
        <td>{{ score.turns }}</td>
        <td>{{ score.time }}</td>
      </tr>
    </table>
    <div class="buttons">
      <div class="button" @click="changePage('Menu')">To Menu</div>
      <div class="button" @click="changePage('Game')">Retry</div>
    </div>
  </div>
</template>

<script>
import Localbase from "localbase";
let db = new Localbase("db");
export default {
  name: "Score",
  data() {
    return {
      scores: [],
    };
  },
  methods: {
    getScores() {
      db.collection("scores")
        .get()
        .then((scores) => {
          this.scores = scores;
        });
    },
    changePage(newPage) {
      this.$emit("changePage", newPage);
    },
  },
  computed: {
    sorted() {
      return this.scores.sort((a, b) => {
        return a.turns - b.turns || a.time - b.time;
      });
    },
  },
  mounted() {
    this.getScores();
  },
};
</script>

<style lang="scss" scoped>
h1 {
  text-align: center;
  font-size: 3em;
  font-weight: bolder;
  color: #fff;
  text-shadow: 1px 2px 3px rgba(23, 0, 0, 0.5);
}
h3 {
  text-align: center;
  color: #fff;
  opacity: 0.8;
  font-size: 1em;
  text-shadow: 1px 2px 1px rgba(23, 0, 0, 0.5);
  margin-bottom: 1em;
}
h2 {
  text-align: center;
  font-family: monospace;
  font-size: 2em;
  text-decoration: underline;
}
table {
  font-family: arial, sans-serif;
  border-collapse: collapse;
  width: 100%;
  margin-top: 1em;
}
td,
th {
  border: 1px solid #dddddd;
  text-align: center;
  padding: 8px;
}
tr:nth-child(even) {
  background-color: #dddddd;
}
.buttons {
  margin-top: 1.5em;
  width: 100%;
  display: flex;
  justify-content: space-around;
  gap: 1em;
  .button {
    text-align: center;
    // margin-left: auto;
    // margin-right: auto;
    // margin-bottom: 0.8em;
    background-color: rgb(241, 239, 239);
    padding: 1.2rem 2em;
    font-size: 1.5rem;
    font-family: monospace;
    box-shadow: 0px 3px 5px #48494b;
    transition: all 0.3s;
    cursor: pointer;
    border-radius: 5px;
    @media (max-width: 760px) {
      width: 90%;
      margin-bottom: 0.5em;
      font-size: 1.2rem;
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
}
</style>