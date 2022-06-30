<template>
  <div class="container">
    <!-- GAME SETTING START -->

    <div v-if="!isHideSettings" class="wrapper">
      <div>
        <div class="game-setting">
          <div>
            <span>Enter Player 1 name!</span>
            <input type="text" v-model="playerOneName" required />
          </div>
        </div>
      </div>
      <button class="start-game-btn" @click="addNames">Start Game</button>
    </div>

    <!-- GAME SETTING END -->

    <!-- PLAYER NAME & SCORE START -->

    <div v-if="!isGameFieldHide" class="player-field">
      <div class="player1">
        <div class="player1-name">{{ player1.name }}</div>
        <div class="player1-score">{{ player1.score }}</div>
      </div>
      <div class="player2">
        <div class="player1-score">{{ player2.score }}</div>
        <div class="player1-name">{{ player2.name }}</div>
      </div>
    </div>

    <!-- PLAYER NAME & SCORE END -->

    <!-- GAME FIELD START -->

    <div v-if="!isGameFieldHide" class="gamefield">
      <button class="rock" @click="choosenRock">
        <img :src="rock" />
      </button>
      <button class="paper" @click="choosenPaper">
        <img :src="paper" />
      </button>
      <button class="scissors" @click="choosenScissors">
        <img :src="scissors" />
      </button>
    </div>

    <!-- GAME FIELD END -->

    <!-- GAME COMMENT START -->

    <div v-if="!isGameFieldHide" class="comment">{{ Result }}</div>
  </div>
  <div class="game-finish" v-if="isGameFinished">
    <p>{{ Result }}</p>
    <button>New Game</button>
  </div>

  <!-- GAME COMMENT END -->
</template>

<script>
import rock from "/src/assets/rock.png";
import paper from "/src/assets/papper.png";
import scissors from "/src/assets/scissors.png";

export default {
  data() {
    return {
      isHideSettings: false,
      playerOneName: "",
      playerTwoName: "Computer",

      rock: rock,
      paper: paper,
      scissors: scissors,

      choiceRock: 0,
      choicePaper: 1,
      choiceScissors: 2,

      Result: "",

      isGameFieldHide: true,
      isGameFinished: false,

      playerOneScore: 0,

      player1: {
        id: 1,
        name: "",
        score: 0,
        choice: 0,
      },
      player2: {
        id: 2,
        name: "Computer",
        score: 0,
        choice: 0,
      },
    };
  },
  watch: {
    player1: {
      score(newValue) {
        console.log(newValue);
        // console.log(oldScore);
        // if (newScore === 3) {
        //   this.isGameFinished = true;
        //   this.Result = "You Won The Game";
        // }
      },
      deep: true,
    },
  },
  methods: {
    addNames() {
      if (this.playerOneName.length === 0) {
        alert("Please enter name !");
        return;
      } else {
        this.player1.name = this.playerOneName;
        this.player2.name = this.playerTwoName;
      }
      this.isHideSettings = !this.isHideSettings;
      this.isGameFieldHide = !this.isGameFieldHide;
    },
    choosenRock() {
      const number = Math.floor(Math.random() * 3);
      if (this.choiceRock === number) {
        this.Result =
          this.player1.name +
          " selected Rock ! " +
          this.player2.name +
          " selected Rock !" +
          " It's Draw.";
      } else if (this.choiceRock + 1 === number) {
        this.Result =
          this.player1.name +
          " selected Rock ! " +
          this.player2.name +
          " selected Paper !" +
          " You loose.";
        this.player2.score++;
      } else {
        this.Result =
          this.player1.name +
          " selected Rock ! " +
          this.player2.name +
          " selected Scissors !" +
          " You Win";
        this.player1.score++;
      }
    },
    choosenPaper() {
      const number = Math.floor(Math.random() * 3);
      if (this.choicePaper === number) {
        this.Result =
          this.player1.name +
          " selected Paper !  " +
          this.player2.name +
          " selected Paper !" +
          "  It's Draw.";
      } else if (this.choicePaper < number) {
        this.Result =
          this.player1.name +
          " selected Paper !  " +
          this.player2.name +
          " selected Scissors !" +
          "  You loose.";
        this.player2.score++;
      } else {
        this.Result =
          this.player1.name +
          " selected Paper !  " +
          this.player2.name +
          " selected Rock !" +
          "  You Win";
        this.player1.score++;
      }
    },
    choosenScissors() {
      const number = Math.floor(Math.random() * 3);
      if (this.choiceScissors === number) {
        this.Result =
          this.player1.name +
          " selected Scissors !  " +
          this.player2.name +
          " selected Scissors !" +
          "  It's Draw.";
      } else if (this.choiceScissors - 1 === number) {
        this.Result =
          this.player1.name +
          " selected Scissors !  " +
          this.player2.name +
          " selected Paper !" +
          "  You Win.";
        this.player1.score++;
      } else {
        this.Result =
          this.player1.name +
          " selected Scissors!  " +
          this.player2.name +
          " selected Rock !" +
          "  You Loose";
        this.player2.score++;
      }
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Indie+Flower&display=swap");

.wrapper {
  width: 800px;
  height: 200px;
  border-radius: 50px;
  background-color: rgb(159, 196, 94);
  text-align: center;
  margin: 0 auto;
  /* position: absolute;
  z-index: 1;
  top: 130px;
  left: 25%;
  box-shadow: 6px 5px 60px 30px black; */
}

.wrapper button {
  margin-top: 20px;
  padding: 7px 25px;
  border-radius: 20px;
  border-color: red;
  background-color: red;
  color: white;
  font-family: "Indie Flower", cursive;
  font-weight: bold;
  font-size: 1.3rem;
  cursor: pointer;
}

.game-setting {
  display: flex;
  align-items: center;
  justify-content: space-evenly;
}

.game-setting span {
  font-family: "Indie Flower", cursive;
  font-size: 1.4rem;
  font-weight: bold;
  padding-right: 10px;
}

.game-setting div {
  text-align: center;
  margin-top: 60px;
}

.game-setting input {
  width: 150px;
  margin: 15px 5px;
  padding: 3px;
  border-radius: 10px;
  border-color: white;
  box-shadow: 0px 2px 5px 0px rgb(58, 58, 58);
}

.player-field {
  width: 800px;
  margin: 0 auto;
  display: flex;
  justify-content: space-between;
}

.player1,
.player2 {
  font-family: "Indie Flower", cursive;
  font-size: 2rem;
  width: 300px;
  display: flex;
  align-content: center;
  justify-content: space-around;
}

.player1-score,
.player2-score {
  font-size: 3rem;
}

.gamefield {
  background-color: rgb(199, 165, 121);
  width: 1200px;
  height: 300px;
  margin: 0 auto;
  display: flex;
  justify-content: space-evenly;
  padding: 50px 0;
  border-radius: 50px 50px 50px 50px;
}

.rock,
.paper,
.scissors {
  padding: 10px;
  cursor: pointer;
}

.gamefield button {
  border-radius: 30px;
  box-shadow: 2px 2px 20px 10px rgba(97, 97, 97, 0.76);
}

.gamefield button:active {
  background-color: rgb(185, 215, 216);
}

.comment {
  font-family: "Indie Flower", cursive;
  font-weight: bolder;
  font-size: 2rem;
  text-shadow: 5px 5px 5px rgb(75, 75, 75);
  text-align: center;
  padding: 20px;
  margin: 20px auto;
  border-radius: 20px;
  width: 70%;
  background-color: rgb(163, 138, 105);
}

.game-finish {
  width: 800px;
  height: 200px;
  margin: 0 auto;
  border-radius: 50px;
  background-color: rgb(159, 196, 94);
  text-align: center;
}

.game-finish p {
  font-family: "Indie Flower", cursive;
  font-size: 4rem;
  padding: 0;
  margin: 15px;
}

.game-finish button {
  margin-top: 10px;
  padding: 7px 25px;
  border-radius: 20px;
  border-color: red;
  background-color: red;
  color: white;
  font-family: "Indie Flower", cursive;
  font-weight: bold;
  font-size: 1.3rem;
  cursor: pointer;
}
</style>