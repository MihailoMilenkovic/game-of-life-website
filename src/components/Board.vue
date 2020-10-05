<template>
  <div>
    <h1 class="header">Conway's game of life</h1>
    <div v-for="(row, rowIdx) in board" v-bind:key="rowIdx" class="table-row">
      <div
        v-for="(square, colIdx) in row"
        v-bind:key="colIdx"
        class="table-cell"
      >
        <Square
          :isAlive="square"
          :row="rowIdx"
          :col="colIdx"
          v-on:clickedSquare="clickedSquare"
        />
      </div>
    </div>
    <StartButton
      v-on:startSimulation="simulate"
      v-bind:style="{ opacity: simulating ? 0 : 1 }"
    />
  </div>
</template>
<script>
import Square from "./Square";
import StartButton from "./StartButton";
export default {
  created() {
    this.generateBoard();
  },
  components: { Square, StartButton },
  name: "Board",
  data: function() {
    return {
      board: [],
      dx: [-1, -1, -1, 0, 0, 1, 1, 1],
      dy: [-1, 0, 1, -1, 1, -1, 0, 1],
      numRows: 10,
      numCols: 15,
      epochs: 50,
      simulating: false,
    };
  },
  methods: {
    generateBoard() {
      let x = Array(this.numRows);
      for (let i = 0; i < x.length; i++) {
        x[i] = Array(this.numCols);
        for (let j = 0; j < x[i].length; j++) {
          x[i][j] = false;
        }
      }
      this.board = x;
    },
    clickedSquare(row, col) {
      if (!this.simulating) {
        this.board[row][col] = !this.board[row][col];
        this.$forceUpdate();
      }
    },
    getAliveNeighbors(row, col) {
      let cnt = 0;
      for (let id = 0; id < this.dx.length; id++) {
        let newX = row + this.dx[id];
        let newY = col + this.dy[id];
        if (
          0 <= newX &&
          newX < this.numRows &&
          0 <= newY &&
          newY < this.numCols
        ) {
          if (this.board[newX][newY]) {
            cnt++;
          }
        }
      }
      return cnt;
    },
    timer(ms) {
      return new Promise((res) => setTimeout(res, ms));
    },
    async simulate() {
      this.simulating = true;
      for (let i = 0; i < this.epochs; i++) {
        let newBoard = [];
        for (let i = 0; i < this.board.length; i++)
          newBoard[i] = this.board[i].slice();

        for (let row = 0; row < this.board.length; row++) {
          for (let col = 0; col < this.board[row].length; col++) {
            let neighbors = this.getAliveNeighbors(row, col);
            if (this.board[row][col]) {
              if (neighbors == 2 || neighbors == 3) {
                newBoard[row][col] = true;
              } else {
                newBoard[row][col] = false;
              }
            } else {
              if (neighbors == 3) {
                newBoard[row][col] = true;
              } else {
                newBoard[row][col] = false;
              }
            }
          }
        }
        this.board = newBoard;
        this.$forceUpdate;
        await this.timer(500);
      }
      this.simulating = false;
    },
  },
};
</script>

<style scoped>
.table-cell {
  margin: 0 2px;
}
.table-row {
  display: flex;
  flex-direction: row;
  justify-content: center;
}
.header {
  margin-bottom: 1rem;
}
</style>
