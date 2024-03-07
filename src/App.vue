<template>
  <div id="app">
    <div class="wrapper clearfix">
      <players-vue
        :isWinner="isWinner"
        :scoresPlayer="scoresPlayer"
        :activePlayer="activePlayer"
        :currentScore="currentScore"
      />
      <controls-vue
        v-on:handleHold="handleHold"
        v-on:handleRollDice="handleRollDice"
        v-on:handleNewGame="handleNewGame"
        v-on:handleFinalScore="handleFinalScore"
        :finalScore="finalScore"
        :isPlaying="isPlaying"
      />
      <dices-vue :dices="dices" />
      <popup-rule-vue v-on:handleConfirm="handleConfirm" :isOpenPopup="isOpenPopup" />
    </div>
  </div>
</template>

<script>
import ControlsVue from './components/Controls.vue';
import DicesVue from './components/Dices.vue';
import PlayersVue from './components/Players.vue';
import PopupRuleVue from './components/PopupRule.vue';

export default {
  name: 'app',
  data() {
    return {
      activePlayer: 0, //ai laf nguoi choi hien tai
      scoresPlayer: [0, 0],
      currentScore: 0,
      isPlaying: false,
      isOpenPopup: false,
      dices: [2, 5],
      finalScore: 10,
    };
  },
  components: {
    PlayersVue,
    ControlsVue,
    DicesVue,
    PopupRuleVue,
  },
  computed: {
    isWinner() {
      let { scoresPlayer, finalScore } = this;

      if (scoresPlayer[0] >= finalScore || scoresPlayer[1] >= finalScore) {
        // Dung cuoc choi
        // this.isPlaying = false;
        return true;
      }
      return false;
    },
  },
  watch: {
    isWinner(newVal) {
      if (newVal) {
        this.isPlaying = false; // Move side effect here
      }
    },
  },
  methods: {
    handleFinalScore(e) {
      var number = parseInt(e.target.value);
      if (isNaN(number)) {
        this.finalScore = '';
      } else {
        this.finalScore = number;
      }
    },
    handleHold() {
      if (this.isPlaying) {
        let { scoresPlayer, activePlayer, currentScore } = this;
        let scoreOld = scoresPlayer[activePlayer];
        let cloneScorePlayer = [...scoresPlayer];
        cloneScorePlayer[activePlayer] = scoreOld + currentScore;
        this.scoresPlayer = cloneScorePlayer;
        if (!this.isWinner) {
          this.nextPlayer();
        }
      } else {
        alert('vui lòng nhấn newgame để bắt đầu  trò chơi');
      }
    },
    nextPlayer() {
      this.activePlayer = this.activePlayer === 0 ? 1 : 0;
      this.currentScore = 0;
    },
    handleRollDice() {
      if (this.isPlaying) {
        var dice1 = Math.floor(Math.random() * 6) + 1;
        var dice2 = Math.floor(Math.random() * 6) + 1;
        this.dices = [dice1, dice2];
        if (dice1 === 1 || dice2 === 1) {
          var activePlayer = this.activePlayer;
          setTimeout(() => {
            alert(`Nguời chơi ${activePlayer + 1} đã quay trúng số 1 , Rất tiếc`);
          }, 10);
          this.nextPlayer();
        } else {
          this.currentScore = this.currentScore + dice1 + dice2;
        }
      } else {
        alert('vui lòng nhấn newgame để bắt đầu  trò chơi');
      }
    },
    handleNewGame() {
      // console.log('tuwf thafng cha');
      this.isOpenPopup = true;
    },
    handleConfirm() {
      // console.log('hihi');
      this.activePlayer = 0;
      this.scoresPlayer = [0, 0];
      this.currentScore = 0;
      this.isOpenPopup = false;
      this.dices = [1, 1];
      this.isPlaying = true;
    },
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
  content: '';
  display: table;
  clear: both;
}

body {
  background-image: linear-gradient(rgba(62, 20, 20, 0.4), rgba(62, 20, 20, 0.4)), url(assets/back.jpg);
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
