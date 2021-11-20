<template>
  <div class="card" :class="{ isFliped: isFliped, isMatched: isMatched }">
    <img src="../../assets/front.jpg" alt="front" />
    <img :src="`/cards/${cardId}.png`" :alt="cardId" />
  </div>
</template>

<script>
export default {
  props: {
    cardId: Number,
    isFliped: {
      type: Boolean,
      default: false,
    },
    isMatched: Boolean,
  },
};
</script>

<style lang="scss" scoped>
.card {
  position: relative;
  width: 130px;
  height: 130px;
  transform-style: preserve-3d;
  perspective: 1000px;
  cursor: pointer;
  @media (max-width: 780px) {
    height: 80px;
    width: 100%;
  }
  @media (max-width: 480px) {
    height: 80px;
    width: 100%;
  }
  &.isFliped {
    pointer-events: none;
  }
}
.card img {
  position: absolute;
  height: 100%;
  width: 100%;
  object-fit: cover;
  backface-visibility: hidden;
  transition: 0.5s;
  border: rgb(10, 9, 9) 2px solid;
}
.card img:nth-child(2) {
  transform: rotateY(180deg);
}
.card.isFliped img:nth-child(2) {
  transform: rotateY(0deg);
}
.card.isFliped.isMatched img:nth-child(2) {
  border: rgb(23, 163, 46) 2px solid;
  opacity: 0.4;
}
.card.isFliped img:nth-child(1) {
  transform: rotateY(-180deg);
}
</style>