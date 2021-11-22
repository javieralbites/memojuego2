<template>
    <h1>The Office</h1>
  <h3>The Memory Game</h3>
  <h2>Credits</h2>
  <h4>Developed by:</h4>
  <a href="http://javieralexander.xyz" target="_blank" rel="noopener noreferrer">
    <img src="https://res.cloudinary.com/ddzblkqfa/image/upload/v1637442182/logo2_mbspkm.png" alt="logo">
  </a>
  <h5 @click="changePage('Menu')">Return to Menu</h5>
</template>

<script>
    export default {
        name: 'Credits',
        methods: {
            changePage(newPage){
                this.$emit("changePage", newPage);
            }
        },
    }
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
h4{
    text-align: center;
    margin-top: 2em;
    margin-bottom: 1em;
}
h5{
    text-align: center;
    margin-top: 3em;
    color: rgb(248, 248, 245);
    text-shadow: 0px 5px 5px grey;
    cursor: pointer;
}
a{
    display: block;
    width: 100%;
    display: flex;
    justify-content: center;
    img{
        filter: invert(1) drop-shadow(5px 5px 5px #222);
        display: block;
    }
}
</style>