<template>
  <div class="container">
    {{ choiceRockTurbo }}
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

    <div v-if="!isScoreFieldHide" class="player-field">
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

    <div v-if="!isResultHide" class="result">
      <div class="player-choice">
        <div v-if="!isPlayerChoiceRock" class="choice">
          <img :src="rock" />
        </div>
        <div v-if="!isPlayerChoicePaper" class="choice">
          <img :src="paper" />
        </div>
        <div v-if="!isPlayerChoiceScissors" class="choice">
          <img :src="scissors" />
        </div>
      </div>
      <div class="computer-choice">
        <div v-if="!isComputerChoiceRock" class="choice">
          <img :src="rock" />
        </div>
        <div v-if="!isComputerChoicePaper" class="choice">
          <img :src="paper" />
        </div>
        <div v-if="!isComputerChoiceScissors" class="choice">
          <img :src="scissors" />
        </div>
      </div>
    </div>

    <!-- GAME FIELD END -->

    <!-- GAME COMMENT START -->

    <div v-if="!isCommentHide" class="comment">
      {{ Result }}
    </div>
  </div>
  <div class="game-finish" v-if="isGameFinished">
    <p>{{ Result }}</p>
    <button @click="startNewGame">New Game</button>
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
      playerOneName: "",
      playerTwoName: "Computer",

      rock: rock,
      paper: paper,
      scissors: scissors,

      playerSelect: "",
      computerSelect: "",

      computerRock: 0,
      computerPaper: 1,
      computerScissors: 2,

      choiceRock: 0,
      choicePaper: 1,
      choiceScissors: 2,

      isResultHide: true,
      isPlayerChoiceRock: false,
      isPlayerChoicePaper: false,
      isPlayerChoiceScissors: false,
      isComputerChoiceRock: false,
      isComputerChoicePaper: false,
      isComputerChoiceScissors: false,
      isHideResult: false,

      Result: "Started the Game! Good Luck!",

      isHideSettings: false,
      isScoreFieldHide: true,
      isCommentHide: true,
      isGameFieldHide: true,
      isGameFinished: false,

      playerOneScore: 0,

      player1: {
        id: 1,
        name: "",
        score: 0,
      },
      player2: {
        id: 2,
        name: "Computer",
        score: 0,
      },
    };
  },
  // computed: {
  //   choiceRockTurbo() {
  //     return this.choiceRock + 5;
  //   },
  // },
  watch: {
    player1: {
      handler(newValue) {
        setTimeout(() => {
          if (newValue.score === 3) {
            this.isGameFinished = true;
            this.isGameFieldHide = true;
            this.isCommentHide = true;
            this.Result = this.player1.name + " Won the Game!";
          }
        }, 2000);
      },
      deep: true,
    },
    player2: {
      handler(newValue) {
        setTimeout(() => {
          if (newValue.score === 3) {
            this.isGameFinished = true;
            this.isGameFieldHide = true;
            this.isCommentHide = true;
            this.Result = this.player2.name + " Won the Game!";
          }
        }, 2000);
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
      }
      this.isHideSettings = true;
      this.isGameFieldHide = false;
      this.isCommentHide = false;
      this.isScoreFieldHide = false;
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
        (this.isPlayerChoicePaper = true),
          (this.isPlayerChoiceScissors = true),
          (this.isComputerChoicePaper = true),
          (this.isComputerChoiceScissors = true);
        this.isGameFieldHide = true;
        this.isResultHide = false;
        setTimeout(() => {
          (this.isPlayerChoicePaper = false),
            (this.isPlayerChoiceScissors = false),
            (this.isComputerChoicePaper = false),
            (this.isComputerChoiceScissors = false);
          this.isGameFieldHide = false;
          this.isResultHide = true;
        }, 2000);
      } else if (this.choiceRock + 1 === number) {
        this.Result =
          this.player1.name +
          " selected Rock ! " +
          this.player2.name +
          " selected Paper !" +
          " You loose.";
        this.isPlayerChoicePaper = true;
        this.isPlayerChoiceScissors = true;
        this.isComputerChoiceRock = true;
        this.isComputerChoiceScissors = true;
        this.isGameFieldHide = true;
        this.isResultHide = false;
        setTimeout(() => {
          this.isPlayerChoicePaper = false;
          this.isPlayerChoiceScissors = false;
          this.isComputerChoiceRock = false;
          this.isComputerChoiceScissors = false;
          this.isGameFieldHide = false;
          this.isResultHide = true;
        }, 2000);
        this.player2.score++;
      } else {
        this.Result =
          this.player1.name +
          " selected Rock ! " +
          this.player2.name +
          " selected Scissors !" +
          " You Win";
        this.isPlayerChoicePaper = true;
        this.isPlayerChoiceScissors = true;
        this.isComputerChoiceRock = true;
        this.isComputerChoicePaper = true;
        this.isGameFieldHide = true;
        this.isResultHide = false;
        setTimeout(() => {
          this.isPlayerChoicePaper = false;
          this.isPlayerChoiceScissors = false;
          this.isComputerChoiceRock = false;
          this.isComputerChoicePaper = false;
          this.isGameFieldHide = false;
          this.isResultHide = true;
        }, 2000);
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
        this.isPlayerChoiceRock = true;
        this.isPlayerChoiceScissors = true;
        this.isComputerChoiceRock = true;
        this.isComputerChoiceScissors = true;
        this.isGameFieldHide = true;
        this.isResultHide = false;
        setTimeout(() => {
          this.isPlayerChoiceRock = false;
          this.isPlayerChoiceScissors = false;
          this.isComputerChoiceRock = false;
          this.isComputerChoiceScissors = false;
          this.isGameFieldHide = false;
          this.isResultHide = true;
        }, 2000);
      } else if (this.choicePaper < number) {
        this.Result =
          this.player1.name +
          " selected Paper !  " +
          this.player2.name +
          " selected Scissors !" +
          "  You loose.";
        this.isPlayerChoiceRock = true;
        this.isPlayerChoiceScissors = true;
        this.isComputerChoiceRock = true;
        this.isComputerChoicePaper = true;
        this.isGameFieldHide = true;
        this.isResultHide = false;
        setTimeout(() => {
          this.isPlayerChoiceRock = false;
          this.isPlayerChoiceScissors = false;
          this.isComputerChoiceRock = false;
          this.isComputerChoicePaper = false;
          this.isGameFieldHide = false;
          this.isResultHide = true;
        }, 2000);
        this.player2.score++;
      } else {
        this.Result =
          this.player1.name +
          " selected Paper !  " +
          this.player2.name +
          " selected Rock !" +
          "  You Win";
        this.isPlayerChoiceRock = true;
        this.isPlayerChoiceScissors = true;
        this.isComputerChoicePaper = true;
        this.isComputerChoiceScissors = true;
        this.isGameFieldHide = true;
        this.isResultHide = false;
        setTimeout(() => {
          this.isPlayerChoiceRock = false;
          this.isPlayerChoiceScissors = false;
          this.isComputerChoicePaper = false;
          this.isComputerChoiceScissors = false;
          this.isGameFieldHide = false;
          this.isResultHide = true;
        }, 2000);
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
        this.isPlayerChoiceRock = true;
        this.isPlayerChoicePaper = true;
        this.isComputerChoiceRock = true;
        this.isComputerChoicePaper = true;
        this.isGameFieldHide = true;
        this.isResultHide = false;
        setTimeout(() => {
          this.isPlayerChoiceRock = false;
          this.isPlayerChoicePaper = false;
          this.isComputerChoiceRock = false;
          this.isComputerChoicePaper = false;
          this.isGameFieldHide = false;
          this.isResultHide = true;
        }, 2000);
      } else if (this.choiceScissors - 1 === number) {
        this.Result =
          this.player1.name +
          " selected Scissors !  " +
          this.player2.name +
          " selected Paper !" +
          "  You Win.";
        this.isPlayerChoiceRock = true;
        this.isPlayerChoicePaper = true;
        this.isComputerChoiceRock = true;
        this.isComputerChoiceScissors = true;
        this.isGameFieldHide = true;
        this.isResultHide = false;
        setTimeout(() => {
          this.isPlayerChoiceRock = false;
          this.isPlayerChoicePaper = false;
          this.isComputerChoiceRock = false;
          this.isComputerChoiceScissors = false;
          this.isGameFieldHide = false;
          this.isResultHide = true;
        }, 2000);
        this.player1.score++;
      } else {
        this.Result =
          this.player1.name +
          " selected Scissors!  " +
          this.player2.name +
          " selected Rock !" +
          "  You Loose";
        this.isPlayerChoiceRock = true;
        this.isPlayerChoicePaper = true;
        this.isComputerChoicePaper = true;
        this.isComputerChoiceScissors = true;
        this.isGameFieldHide = true;
        this.isResultHide = false;
        setTimeout(() => {
          this.isPlayerChoiceRock = false;
          this.isPlayerChoicePaper = false;
          this.isComputerChoicePaper = false;
          this.isComputerChoiceScissors = false;
          this.isGameFieldHide = false;
          this.isResultHide = true;
        }, 2000);
        this.player2.score++;
      }
    },
    startNewGame() {
      this.player1.score = 0;
      this.player2.score = 0;
      this.Result = this.player1.name + " selected New Game. Good Luck !";
      this.isGameFinished = false;
      this.isGameFieldHide = false;
      this.isCommentHide = false;
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Indie+Flower&display=swap");

.wrapper {
  width: 50%;
  height: 200px;
  border-radius: 50px;
  background-color: rgb(159, 196, 94);
  text-align: center;
  margin: 0 auto;
}

.wrapper button {
  width: 20%;
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
  width: 40%;
  margin: 15px 5px;
  padding: 3px;
  border-radius: 10px;
  border-color: white;
  box-shadow: 0px 2px 5px 0px rgb(58, 58, 58);
}

.player-field {
  width: 50%;
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
  width: 60%;
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

.result {
  display: flex;
  justify-content: space-evenly;
  align-content: center;
  width: 60%;
  height: 250px;
  background-color: rgb(199, 165, 121);
  border-radius: 50px 50px 50px 50px;
  padding: 50px 0;
  margin: 0 auto;
}

.choice img {
  width: 250px;
  height: 250px;
}

.comment {
  font-family: "Indie Flower", cursive;
  font-weight: bolder;
  font-size: 2rem;
  color: aqua;
  text-shadow: 5px 5px 5px rgb(104, 104, 104);
  text-align: center;
  padding: 20px;
  margin: 20px auto;
  border-radius: 20px;
  width: 70%;
  background-color: rgb(163, 138, 105);
}

.game-finish {
  width: 50%;
  height: 250px;
  margin: 0 auto;
  border-radius: 50px;
  background-color: rgb(159, 196, 94);
  text-align: center;
}

.game-finish p {
  font-family: "Indie Flower", cursive;
  font-size: 2rem;
  margin-top: 0;
  padding-top: 30px;
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

/* Extra small devices (phones, 600px and down) */
@media only screen and (max-width: 600px) {
  .container {
    margin: 0;
    padding: 0;
  }
  .wrapper {
    width: 100%;
    height: 280px;
  }
  .wrapper button {
    width: 60%;
  }
  .player-field {
    gap: 40px;
    font-weight: bold;
    margin: 0 auto;
  }

  .gamefield {
    width: 100%;
    margin: 0;
  }

  .gamefield button {
    width: 95px;
    height: 95px;
    box-shadow: 2px 2px 5px 2px rgba(97, 97, 97, 0.76);
  }

  .gamefield img {
    width: 75px;
    height: 75px;
  }

  .player1,
  .player2,
  .player1-score,
  .player2-score {
    width: 20%;
    font-size: 1rem;
    padding: 1rem;
  }
  .result {
    display: flex;
    justify-content: space-evenly;
    align-content: center;
    width: 100%;
    height: 90px;
    background-color: rgb(199, 165, 121);
    border-radius: 50px 50px 50px 50px;
    padding: 40px 0;
    margin: 0 auto;
  }

  .choice img {
    width: 110px;
    height: 110px;
  }

  .comment {
    width: 80%;
    font-size: 1rem;
  }
  .game-finish {
    width: 90%;
    height: 150px;
  }
  .game-finish p {
    padding-top: 10px;
    font-weight: bold;
    font-size: 1rem;
  }
}

/* Small devices (portrait tablets and large phones, 600px and up) */
@media only screen and (min-width: 600px) {
  .wrapper {
    width: 80%;
    height: 280px;
  }
  .wrapper button {
    font-size: 1.5rem;
    width: 25%;
  }
  .player-field {
    gap: 40px;
    font-weight: bold;
    margin: 0 auto;
  }
  .player1,
  .player2,
  .player1-score,
  .player2-score {
    width: 20%;
    font-size: 2rem;
    padding: 1rem;
  }
  .gamefield {
    width: 80%;
    margin: 0 auto;
  }

  .gamefield button {
    width: 110x;
    height: 110px;
    box-shadow: 2px 2px 5px 2px rgba(97, 97, 97, 0.76);
  }

  .gamefield img {
    width: 85px;
    height: 85px;
  }
  .result {
    display: flex;
    justify-content: space-evenly;
    align-content: center;
    width: 80%;
    height: 270px;
    background-color: rgb(199, 165, 121);
    border-radius: 50px 50px 50px 50px;
    padding: 40px 0;
    margin: 0 auto;
  }

  .choice img {
    width: 250px;
    height: 250px;
  }

  .game-finish {
    width: 80%;
    height: 200px;
  }
  .game-finish p {
    padding-top: 20px;
    font-weight: bold;
    font-size: 1.5rem;
  }
}

/* Medium devices (landscape tablets, 768px and up) */
/* @media only screen and (min-width: 768px) {
} */

/* Large devices (laptops/desktops, 992px and up) */
/* @media only screen and (min-width: 992px) {
  ...;
} */

/* Extra large devices (large laptops and desktops, 1200px and up) */
@media only screen and (min-width: 1200px) {
  .game-setting div {
    margin-top: 30px;
  }

  .wrapper {
    width: 50%;
    height: 200px;
  }
  .wrapper input {
    width: 250px;
    padding: 0.4rem;
  }
  .wrapper span {
    font-size: 1.5rem;
  }
  .wrapper button {
    font-size: 1.5rem;
    width: 20%;
  }
  .player-field {
    margin: 0 auto;
    font-weight: bold;
    justify-content: space-around;
  }
  .player1,
  .player2 {
    justify-content: space-between;
  }
  .player1-score,
  .player2-score {
    font-size: 3rem;
  }
  .gamefield {
    width: 60%;
    height: 250px;
    margin: 0 auto;
  }

  .gamefield button {
    width: 230px;
    height: 230px;
    box-shadow: 2px 2px 30px 10px rgba(97, 97, 97, 0.76);
  }
  .gamefield img {
    width: 170px;
    height: 170px;
  }
  .result {
    display: flex;
    justify-content: space-evenly;
    align-content: center;
    width: 60%;
    height: 270px;
    background-color: rgb(199, 165, 121);
    border-radius: 50px 50px 50px 50px;
    padding: 40px 0;
    margin: 0 auto;
  }

  .choice img {
    width: 250px;
    height: 250px;
  }
  .game-finish {
    width: 50%;
    height: 200px;
  }
  .game-finish p {
    padding-top: 10px;
    font-weight: bold;
    font-size: 2.3rem;
  }
}
</style>