<template>
  <h1>Rock - Paper - Scissors</h1>
  <game-field></game-field>
</template>

<script>
import GameField from "./components/GameField.vue";
export default {
  data() {
    return {
      rock: "./assets/rock.png",
      paper: "./assets/papper.png",
      scissors: "./assets/scissors.png",
    };
  },
  components: { GameField },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Lobster&display=swap");

h1 {
  text-align: center;
  font-family: "Lobster", cursive;
  font-size: 3.5rem;
}

@media only screen and (max-width: 600px) {
  h1 {
    font-size: 2rem;
  }
}
</style>>
