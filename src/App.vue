<template>
  <div class="container">
    <!-- <transition name="component-fade" mode="out-in"> -->
      <component :is="actualPage" @changePage="changePage"></component>
    <!-- </transition> -->
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

<style>
.component-fade-enter-active,
.component-fade-leave-active {
  transition: opacity 0.3s ease;
}
.component-fade-enter-from,
.component-fade-leave-to {
  opacity: 0;
}
</style>