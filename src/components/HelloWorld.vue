<template>
  <h2>Tic-Tac-Toe</h2>
  <h3 v-if="!gameStage">Current Player : {{ currentPlayer ? "X" : "0" }}</h3>
  <h3 v-else>Player {{ winner }} Won the Match</h3>
  <h3 v-if="count > matrixSize * matrixSize - 1 && !gameStage" style="color: red">
    No one is Won
  </h3>

  <div class="container">
    <div class="player-card">
      <h1>Player X</h1>
      <h2>{{ winnerXCount }}</h2>
      <div class="next-turn text-center" v-if="currentPlayer">
      </div>
    </div>
    <div class="board-container" v-if="matrix.length > 0">
      <div class="board" id="boardElementId">
        <div v-for="(matrixRow, index) in matrix" :key="index">
          <div v-for="(col, colIndex) in matrixRow" :key="colIndex">
            <div class="cell" id="cellAction" @click="setValue(colIndex, index)">
              {{ matrix[colIndex][index] !== -1 ? matrix[colIndex][index] : "" }}
            </div>
          </div>
        </div>
      </div>
      <div class="btn-container">
        <button class="btn" @click="nextGame()">Next Game</button>
        <button class="btn" @click="startNewGame()">Start New Game</button>
      </div>
    </div>
    <div class="player-card">
      <h1>Player 0</h1>
      <h2>{{ winnerYCount }}</h2>
      <div class="next-turn text-center" v-if="!currentPlayer">
        <!-- <h1>Your Turn</h1> -->
      </div>
    </div>
  </div>
  <div class="container-min">
    <div class="player-card">
      <h1>Player X</h1>
      <h2>{{ winnerXCount }}</h2>
      <div class="next-turn text-center" v-if="currentPlayer">
        <!-- <h1>Your Turn</h1> -->
      </div></div>
      <div class="player-card">
        <h1>Player 0</h1>
        <h2>{{ winnerYCount }}</h2>
        <div class="next-turn text-center" v-if="!currentPlayer">
          <!-- <h1>Your Turn</h1> -->
        </div>
      </div>
    
  </div>
</template>

<script>
export default {
  data() {
    return {
      name: "HelloWorld",
      matrix: [],
      matrixSize: 3,
      currentPlayer: true,
      winner: "",
      gameStage: 0,
      winnerXCount: 0,
      winnerYCount: 0,
      count: 0,
    };
  },
  mounted() {
    this.generateMatrix();
  },

  methods: {
    generateMatrix() {
      this.matrix = [];
      (this.winner = ""), (this.gameStage = 0), (this.count = 0);
      for (let i = 0; i < this.matrixSize; i++) {
        const newMatrix = Array.from({ length: this.matrixSize }, () => -1);
        this.matrix.push(newMatrix);
      }

      const elements = document.getElementsByClassName("board");
      if (this.matrixSize > 6) {
        for (let i = 0; i < elements.length; i++) {
          const element = elements[i];
          // element.style.width = Math.floor(this.matrixSize/1.5) + '00px';
          // element.style.height = Math.floor(this.matrixSize/2.5) + '00px';

          // element.style.width = this.matrixSize + '00px';
          // element.style.height = this.matrixSize + '00px';
          element.style.gridTemplateColumns = "repeat(" + this.matrixSize + ", 1fr)";
          element.style.gridTemplateRows = "repeat(" + this.matrixSize + ", 1fr)";
        }
        const cells = document.querySelectorAll(".cell");

        // const cellSize = Math.floor((this.matrixSize/1.5) * 100 / this.matrixSize);
        const cellSize = Math.floor(600 / this.matrixSize);

        cells.forEach((cell) => {
          cell.style.width = cellSize + "px";
          cell.style.height = cellSize + "px";
        });
      } else {
        for (let i = 0; i < elements.length; i++) {
          const element = elements[i];
          element.style.width = this.matrixSize + "00px";
          element.style.height = this.matrixSize + "00px";
          element.style.gridTemplateColumns = "repeat(" + this.matrixSize + ", 1fr)";
          element.style.gridTemplateRows = "repeat(" + this.matrixSize + ", 1fr)";
        }

        const cellSize = (this.matrixSize * 100) / this.matrixSize;
        const cells = document.querySelectorAll(".cell");

        cells.forEach((cell) => {
          cell.style.width = cellSize + "px";
          cell.style.height = cellSize + "px";
        });
      }
    },
    setValue(r, c) {
      if (!this.gameStage && this.count < 9 && this.matrix[r][c] == -1) {
        this.count++;
        this.matrix[r][c] = this.currentPlayer ? "X" : "0";
        this.currentPlayer = !this.currentPlayer;
        this.checkValid("X");
        this.checkValid("0");
      }
    },
    checkValid(player) {
      for (let i = 0; i < this.matrix.length; i++) {
        let row = true;
        let col = true;
        for (let j = 0; j < this.matrix.length; j++) {
          if (this.matrix[i][j] !== player) {
            row = false;
          }
          if (this.matrix[j][i] !== player) {
            col = false;
          }
        }
        if (row || col) {
          this.gameStage = 1;
          this.winner = player;
          player == "X" ? this.winnerXCount++ : this.winnerYCount++;
          return true;
        }
      }

      let diag1Win = true;
      let diag2Win = true;
      for (let i = 0; i < this.matrix.length; i++) {
        if (this.matrix[i][i] !== player) {
          diag1Win = false;
        }
        if (this.matrix[i][this.matrix.length - i - 1] !== player) {
          diag2Win = false;
        }
      }
      if (diag1Win || diag2Win) {
        this.gameStage = 1;
        this.winner = player;
        player == "X" ? this.winnerXCount++ : this.winnerYCount++;
        return true;
      }
    },

    nextGame() {
      this.matrix = [];
      (this.winner = ""), (this.gameStage = 0), (this.count = 0);
      this.generateMatrix();
    },
    startNewGame() {
      this.matrix = [];
      (this.winner = ""),
        (this.gameStage = 0),
        (this.winnerXCount = 0),
        (this.winnerYCount = 0),
        (this.count = 0),
        this.generateMatrix();
    },
    setActivePlayer() {
      document.getElementById("cellAction").addEventListener("click", function () {
        // Remove 'active' class from all users
        document.querySelectorAll(".player-x").forEach((item) => {
          item.classList.remove("active");
        });

        // Add 'active' class to User 1
        document.getElementsByClassName("player-x").classList.add("active");
      });
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  display: flex;
  justify-content: space-around;
}

.player-card {
  min-width: 18vw;
}

.player-card h1 {
  padding: 20px;
  margin: 10px;
  border: 2px solid #ccc;
  border-radius: 10px;
  background-color: #f9f9f9;
  transition: background-color 0.3s, border-color 0.3s, box-shadow 0.3s;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
.player-x.active {
  color: #057fe8;
}

.player-y.active {
  color: #057fe8;
}

.board {
  width: 300px;
  height: 300px;
  display: flex;
  flex-direction: column;
  max-width: 100%;
  margin: 20px auto;
  text-align: center;
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 1fr);
}

.cell {
  justify-content: center;
  align-items: center;
  display: flex;
  width: 100px;
  height: 100px;
  border: 1px solid #000000;
  font-size: 24px;
  cursor: pointer;
  background-color: #c5c2bf;
  box-sizing: border-box;
}

.btn-container {
  margin: auto;
  display: flex;
  justify-content: center;
}

.board-container {
  display: flex;
  flex-direction: column;
}

.btn {
  display: inline-block;
  margin-top: 20px;
  background-color: #29292d;
  cursor: pointer;
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 0 40px 0 40px;
}
.container-min {
  display: none;
}
@media (max-width: 800px) {
  .container {
    flex-direction: column;
  }

  .board-container {
    order: 1;
  }

  .player-card {
    order: 2;
    margin-top: 10px;
    display: none;
  }

  .btn {
    padding: 9px 20px;
    margin: 10px;
  }
   .board {
       margin: 12px auto;
  }

  /* .cell {
    width: 70px;
    height: 70px;
  } */
  .container-min {
    display: flex;
    justify-content: space-around;
  }
  .container-min .player-card {
    display: flex;
     flex-direction: column;
   
   
  }
  .player-card h1{
    font-size: 17px;
  }
}
</style>
