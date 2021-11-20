<template>
  <div class="menu">
    <h1>The Office</h1>
    <h3>The Memory Game</h3>
    <div class="btnmenu" @click="changePage('Game')">Play</div>
    <div class="btnmenu" @click="changePage('Score')">Scores</div>
    <div class="btnmenu" @click="changePage('Credits')">Credits</div>
  </div>
</template>

<script>
export default {
  methods: {
    name: "Menu",
    changePage(newPage) {
      this.$emit("changePage", newPage);
    },
  },
};
</script>

<style lang="scss" scoped>
.menu {
  text-align: center;
  h1 {
    font-size: 5em;
    font-weight: bolder;
    color: #fff;
    text-shadow: 1px 2px 3px rgba(23, 0, 0, 0.5);
  }
  h3 {
    color: #fff;
    opacity: 0.8;
    text-shadow: 1px 2px 1px rgba(23, 0, 0, 0.5);
    margin-bottom: 1em;
  }
  .btnmenu {
    width: 50%;
    margin-left: auto;
    margin-right: auto;
    margin-bottom: 0.8em;
    background-color: rgb(241, 239, 239);
    padding: 1.2rem;
    font-size: 3rem;
    font-family: monospace;
    box-shadow: 0px 5px 10px #48494b;
    transition: all 0.3s;
    cursor: pointer;
    border-radius: 5px;
    @media (max-width: 760px) {
      width: 90%;
      margin-bottom: 0.5em;
    }
    &:hover {
      box-shadow: 0px 15px 25px -5px #48494bee;
      transform: scale(1.02);
    }
    &:active {
      box-shadow: 0px 4px 8px #48494bee;
      transform: scale(0.98);
    }
  }
}
</style>