<template>
  <div class="game">
    <div class="about-link">
      <router-link to="hakkimizda">Hakkımızda</router-link>
    </div>
    <table cellspacing="0">
      <tr v-bind:key="rowIndex" v-for="(row, rowIndex) in board">
          <td v-bind:key="columnIndex" v-for="(column, columnIndex) in row"
            v-on:click="cellClick(rowIndex, columnIndex)">
            <transition name="fade">
              <span v-if="board[rowIndex][columnIndex]">{{board[rowIndex][columnIndex]}}</span>
            </transition>
          </td>
      </tr>
    </table>
    <br>
    <transition name="fade">
      <button v-if="!isNewGame" class="reset-button" v-on:click="reset()">Yeniden Başla</button>
    </transition>
    <transition name="fade">
      <h1 v-if="winner">{{winner}} Kazandı!</h1>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'Game',
  data() {
    return {
      board: [
        ['', '', ''],
        ['', '', ''],
        ['', '', ''],
      ],
      winner: '',
      turn: 'X',
      isNewGame: true,
    };
  },
  methods: {
    cellClick: function cellClick(x, y) {
      if (this.winner) return;
      if (!this.board[x][y]) {
        this.isNewGame = false;
        this.board[x][y] = this.turn;
        if (this.turn === 'X') {
          this.turn = 'O';
        } else {
          this.turn = 'X';
        }
      }
      this.$forceUpdate();

      if (this.isDone()) {
        this.gameOverMessage = `${this.winner} Kazandı!`;
      }
    },
    reset: function reset() {
      this.board = [['', '', ''], ['', '', ''], ['', '', '']];
      this.winner = null;
      this.turn = 'X';
      this.isNewGame = true;
    },

    isDone: function isDone() {
      // Yatay ve dikey durumlar kontrol ediliyor.
      for (let i = 0; i < 3; i++) {
        let patternHorizontal = '';
        let patternVertical = '';
        for (let j = 0; j < 3; j++) {
          patternHorizontal += this.board[i][j];
          patternVertical += this.board[j][i];
        }
        if (patternVertical === 'XXX' || patternHorizontal === 'XXX') {
          this.winner = 'X';
          return true;
        } else if (patternVertical === 'OOO' || patternHorizontal === 'OOO') {
          this.winner = 'O';
          return true;
        }
      }

      // Çapraz durumlar kontrol ediliyor.
      const leftDiagonalPattern = this.board[0][0] + this.board[1][1] + this.board[2][2];
      const rightDiagonalPattern = this.board[0][2] + this.board[1][1] + this.board[2][0];
      if (leftDiagonalPattern === 'XXX' || rightDiagonalPattern === 'XXX') {
        this.winner = 'X';
        return true;
      } else if (leftDiagonalPattern === 'OOO' || rightDiagonalPattern === 'OOO') {
        this.winner = 'O';
        return true;
      }

      // Tüm alanlar dolu mu diye kontrol ediliyor, beraberlik durumu.
      for (let i = 0; i < 3; i++) {
        for (let j = 0; j < 3; j++) {
          if (!this.board[i][j]) {
            return false;
          }
        }
      }
      this.winner = 'Dostluk';
      return true;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

table{
  margin: 0 auto;
}

td{
  font-size: 30px;
  color: #757575;
  width: 60px;
  height: 60px;
  line-height: 60px;
  text-align: center;
}

tr:first-child td,
tr:nth-child(2) td{
  border-bottom: 1px solid #ccc;
}

tr td:first-child,
tr td:nth-child(2){
  border-right: 1px solid #ccc;
}

.fade-leave-active{
  transition: opacity .3s;
}
.fade-enter-active {
  transition: opacity 1.5s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}

.reset-button {
  border: none;
  background-color: #000000;
  color: white;
  padding: 10px;
  margin-top: 20px;
}

.about-link{
  margin-bottom: 40px;
}
</style>
