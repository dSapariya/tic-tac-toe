<template>
  <h1>Tic-tac-toe</h1>
  <!-- <label>Enter Matrix Size:</label>
  <input v-model="matrixSize" type="Number" min="3" max="10" @change="generateMatrix()" /> -->
  <h3 v-if="!gameStage">Current Planyer : {{ currentPlanyer ? 'X' : '0' }} </h3>
  <h3 v-else>Player {{ winner }} Won the Match</h3>
  <h3 v-if="count > (matrixSize*matrixSize)-1 && !gameStage" style="color: red;">No one is Won</h3>
  <div class="container">
    <div class="player-x">
      <h1>Player X</h1>
      <h2>{{ winnerXCount }}</h2>
    </div>
    <div class="board-container" v-if="matrix.length > 0">
      <div class="board" id="boardElementId">
        <div v-for="(matrixRow, index) in matrix" :key="index">
          <div v-for="(col, colIndex) in matrixRow" :key="colIndex">
            <div class="cell" id="cellAction" @click="setValue(colIndex, index)">
              {{ matrix[colIndex][index] !== -1 ? matrix[colIndex][index] : '' }}
            </div>
          </div>
        </div>
      </div>
      <div id="container"></div>
      <div class="btn-container">
        <button class="btn" @click="nextGame()">Next Game</button>
        <button class="btn" @click="startNewGame()">Start New Game</button>
      </div>
    </div>
    <div class="player-y">
      <h1>Player 0</h1>
      <h2>{{ winnerYCount }}</h2>
    </div>
  </div>



</template>

<script>
export default {
  data() {
    return {
      name: 'HelloWorld',
      matrix: [],
      matrixSize: 3,
      currentPlanyer: true,
      winner: '',
      gameStage: 0,
      winnerXCount: 0,
      winnerYCount: 0,
      count: 0
    }
  },
  mounted() {
    this.generateMatrix();


     
  },
  methods: {

    generateMatrix() {
      this.matrix = [];
      this.winner = '',
        this.gameStage = 0,
        // this.winnerXCount = 0,
        // this.winnerYCount = 0,
        this.count = 0
      console.log('matr', this.matrixSize);
      for (let i = 0; i < this.matrixSize; i++) {

        const newMatrix = Array.from({ length: this.matrixSize }, () => -1);
        console.log('newMatrix', newMatrix)
        this.matrix.push(newMatrix)
      }

      const elements = document.getElementsByClassName('board');
      console.log(' const maxWidth = container.clientWidth;', elements.clientWidth)
      console.log('elements.length',elements.length)
      if (this.matrixSize > 6) {
        console.log('elements.length----',elements.length)
        for (let i = 0; i < elements.length; i++) {
          console.log('ele', elements[i].style)
          const element = elements[i];
           console.log('element.style', element.style.width)
          // element.style.width = Math.floor(this.matrixSize/1.5) + '00px';
          // element.style.height = Math.floor(this.matrixSize/2.5) + '00px';

          // element.style.width = this.matrixSize + '00px';
          // element.style.height = this.matrixSize + '00px';
          element.style.gridTemplateColumns = 'repeat(' + this.matrixSize + ', 1fr)';
          element.style.gridTemplateRows = 'repeat(' + this.matrixSize + ', 1fr)';
        }
        const cells = document.querySelectorAll('.cell');

        // const cellSize = Math.floor((this.matrixSize/1.5) * 100 / this.matrixSize);
        const cellSize = Math.floor(600 / this.matrixSize);

        console.log('containerWidth', cellSize)

        console.log('cell', cells)
        cells.forEach(cell => {
          cell.style.width = cellSize + 'px';
          cell.style.height = cellSize + 'px';
        });


      }
      else {
        for (let i = 0; i < elements.length; i++) {
          console.log('ele', elements[i].style)
          const element = elements[i];
            console.log('element.style', element.style.width)
          element.style.width = this.matrixSize + '00px';
          element.style.height = this.matrixSize + '00px';
          element.style.gridTemplateColumns = 'repeat(' + this.matrixSize + ', 1fr)';
          element.style.gridTemplateRows = 'repeat(' + this.matrixSize + ', 1fr)';
        }

        const cellSize = this.matrixSize * 100 / this.matrixSize;
        const cells = document.querySelectorAll('.cell');
        console.log('containerWidth', cellSize)

        console.log('cell', cells)
        cells.forEach(cell => {
          cell.style.width = cellSize + 'px';
          cell.style.height = cellSize + 'px';
        });


      }

    },
    setValue(r, c) {
      if (!this.gameStage && this.count < 9 && this.matrix[r][c] == -1) {
       
        this.count++;
        this.matrix[r][c] = this.currentPlanyer ? 'X' : '0'
        this.currentPlanyer = !this.currentPlanyer
        this.checkValid('X');
        this.checkValid('0');
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
            col = false
          }
        }
        if (row || col) {
          console.log('WINNER T')
          this.gameStage = 1
          this.winner = player
          player == 'X' ? this.winnerXCount++ : this.winnerYCount++;
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
        this.gameStage = 1
        this.winner = player
        player == 'X' ? this.winnerXCount++ : this.winnerYCount++;
        return true;
      }
    },

    nextGame() {
      this.matrix = [];
      this.winner = '',
      this.gameStage = 0,
      this.count = 0;
      this.generateMatrix();
    },
    startNewGame() {
      this.matrix = [];
      this.winner = '',
        this.gameStage = 0,
        this.winnerXCount = 0,
        this.winnerYCount = 0,
        this.count = 0,
        this.generateMatrix();
    },
    setActivePlayer(){
      document.getElementById('cellAction').addEventListener('click', function() {
    // Remove 'active' class from all users
    console.log('active');
    document.querySelectorAll('.player-x').forEach(item => {
      console.log('dwe');
        item.classList.remove('active');
    });

    // Add 'active' class to User 1
    document.getElementsByClassName('player-x').classList.add('active');
});
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container {
  display: flex;
  justify-content: space-between;
}

.player-x {
  width: 20%;
}
.player-x.active {
       color: #057fe8;
} 
.player-y {
  width: 20%;
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
  display: flex;
  justify-content: space-between;
  margin-top: 20px;
}


.board-container {
  display: flex;
  flex-direction: column;

}

.btn {
  padding: 10px;
  font-size: 20px;
}



</style>
