<template>
  <h1>SCORE</h1>
  <div class="table">
    <div v-for="(score, index) in sorted" :key="index">
      {{ index + 1 }} - {{ score.player }} - {{ score.turns }} -{{ score.time }}
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