<template>
  <div class="container">
    <component :is="actualPage" @changePage="changePage"></component>
  </div>
</template>

<script>
import Menu from "./components/pages/Menu.vue";
import Score from "./components/pages/Score.vue";
import Credits from "./components/pages/Credits.vue";
import Game from "./components/pages/Game.vue";
export default {
  components: {
    Menu,
    Score,
    Credits,
    Game,
  },
  data() {
    return {
      actualPage: "Menu",
    };
  },
  methods: {
    changePage(newPage) {
      this.actualPage = newPage;
    },
  },
};
</script>
