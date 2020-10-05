<template>
  <div class="wrapper clearfix">
    <players
      v-bind:isWinner="isWinner"
      v-bind:scorePlayers="scorePlayers"
      v-bind:currentScore="currentScore"
      v-bind:currentPlayer="currentPlayer"
    />
    <controls
      v-bind:isPlaying="isPlaying"
      v-on:handleChangeFinalScore="handleChangeFinalScore"
      v-bind:finalScore="finalScore"
      v-on:handleHoldScore="handleHoldScore"
      v-on:handleRollDice="handleRollDice"
      v-on:handleNewGame="handleNewGame"
    />
    <dices v-bind:dices="dices" />

    <popup-rule
      v-on:handleConfirm="handleConfirm"
      v-bind:isOpenpopup="isOpenPopup"
    />
  </div>
</template>

<script>
import Players from "./components/Players.vue";
import Controls from "./components/Controls";
import Dices from "./components/Dices";
import PopupRule from "./components/PopupRule";

export default {
  name: "app",
  data() {
    return {
      scorePlayers: [0, 0],
      currentScore: 0,
      currentPlayer: 0,
      isPlaying: false,
      isOpenPopup: false,
      dices: [5, 5],
      finalScore: 10,
    };
  },
  computed: {
    isWinner() {
      let { scorePlayers, finalScore } = this;
      if (scorePlayers[0] >= finalScore || scorePlayers[1] >= finalScore) {
        this.isPlaying = false;
        return true;
      }
      return false;
    },
  },
  methods: {
    handleNewGame() {
      this.isOpenPopup = true;
      this.isPlaying = true;
    },
    handleConfirm() {
      this.isOpenPopup = false;
      this.scorePlayers = [0, 0];
      this.currentScore = 0;
      this.dices = [1, 1];
    },
    handleRollDice() {
      if (this.isPlaying) {
        let dice1 = Math.floor(Math.random() * 6) + 1;
        let dice2 = Math.floor(Math.random() * 6) + 1;

        this.dices = [dice1, dice2];

        if (dice1 == 1 || dice2 == 1) {
          let Player = this.currentPlayer;
          setTimeout(() => {
            alert(`Nguoi choi ${Player} da xoay trung so 1, rat tiec!`);
          }, 10);

          this.currentPlayer = this.currentPlayer === 0 ? 1 : 0;
          this.currentScore = 0;
        } else {
          this.currentScore += dice1 + dice2;
        }
      } else {
        alert("Vui lòng nhấn New Game");
      }
    },
    handleHoldScore() {
      if (this.isPlaying) {
        const { currentScore, currentPlayer, scorePlayers } = this;
        const scoreOld = scorePlayers[currentPlayer];
        let cloneScorePlayer = [...scorePlayers];
        cloneScorePlayer[currentPlayer] = scoreOld + currentScore;
        this.scorePlayers = cloneScorePlayer;

        if (!this.isWinner) {
          this.currentPlayer = this.currentPlayer === 0 ? 1 : 0;
          this.currentScore = 0;
        }
      } else {
        alert("Vui lòng nhấn New Game");
      }
    },
    handleChangeFinalScore(e) {
      let number = parseInt(e.target.value);
      if (isNaN(number)) {
        this.finalScore = "";
      } else {
        this.finalScore = number;
      }
    },
  },
  components: {
    Players,
    Controls,
    Dices,
    PopupRule,
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.clearfix::after {
  content: "";
  display: table;
  clear: both;
}

body {
  background-image: linear-gradient(
      rgba(62, 20, 20, 0.4),
      rgba(62, 20, 20, 0.4)
    ),
    url("/public/assets/back.jpg");
  background-size: cover;
  background-position: center;
  font-family: Lato;
  font-weight: 300;
  position: relative;
  height: 100vh;
  color: #555;
}

.wrapper {
  width: 1000px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: #fff;
  box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
  overflow: hidden;
}
</style>
