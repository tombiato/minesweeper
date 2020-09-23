<template>
  <div id="grid">
    <Square v-for="square in squares"
            :key="square.id"
            :id="square.id"
            :classItem="square.class"
            :checked="square.checked"
            :flag="square.flag"
            :text="square.text"
            :total="square.total"
            @click="click"
            @contextMenu="addFlag"/>
  </div>
</template>

<script>
import Square from "@/components/Square";

export default {
  name: "Grid",
  components: {
    Square
  },
  data() {
    return {
      width: 10,
      gridSize: this.width * this.width,
      squares: [],
      bombAmount: 20,
      flags: 0,
      isGameOver: false
    }
  },
  methods: {
    // Init the base grid
    createGrid: function () {
      const bombArray = Array(this.bombAmount).fill('bomb');
      const emptyArray = Array(this.width * this.width - this.bombAmount).fill('valid');
      const gameArray = emptyArray.concat(bombArray);

      // Nouveau tableau random
      const shuffleArray = gameArray.sort(() => Math.random() - 0.5);

      for (let i = 0; i < this.width * this.width; i++) {
        let square = {
          id: i,
          class: shuffleArray[i],
          checked: '',
          flag: '',
          text: '',
          total: 0
        }
        this.squares.push(square);
      }

      // Ajout des numeros
      for (let i = 0; i < this.squares.length; i++) {
        const isLeftEdge = (i % this.width === 0);
        const isRightEdge = (i % this.width === this.width - 1);
        let total = 0;

        if (this.squares[i].class === 'valid') {
          if (i > 0 && !isLeftEdge && this.squares[i - 1].class === 'bomb') {
            total++;
          }
          if (i > 9 && !isRightEdge && this.squares[i + 1 - this.width].class === 'bomb') {
            total++;
          }
          if (i > 10 && this.squares[i - this.width].class === 'bomb') {
            total++;
          }
          if (i > 11 && !isLeftEdge && this.squares[i - 1 - this.width].class === 'bomb') {
            total++;
          }
          if (i < 98 && !isRightEdge && this.squares[i + 1].class === 'bomb') {
            total++;
          }
          if (i < 90 && !isLeftEdge && this.squares[i - 1 + this.width].class === 'bomb') {
            total++;
          }
          if (i < 88 && !isRightEdge && this.squares[i + 1 + this.width].class === 'bomb') {
            total++;
          }
          if (i < 89 && this.squares[i + this.width].class === 'bomb') {
            total++;
          }
          this.squares[i].total = total;
        }
      }
    },

    // Check recursively squares around a position
    checkSquare: function (id) {
      const isLeftEdge = (id % this.width === 0)
      const isRightEdge = (id % this.width === this.width - 1)

      setTimeout(() => {
        if (id > 0 && !isLeftEdge) {
          const newId = this.squares[parseInt(id) - 1].id
          const newSquare = this.squares[newId]
          this.click(newSquare.id)
        }
        if (id > 9 && !isRightEdge) {
          const newId = this.squares[parseInt(id) + 1 - this.width].id
          const newSquare = this.squares[newId]
          this.click(newSquare.id)
        }
        if (id > 10) {
          const newId = this.squares[parseInt(id - this.width)].id
          const newSquare = this.squares[newId]
          this.click(newSquare.id)
        }
        if (id > 11 && !isLeftEdge) {
          const newId = this.squares[parseInt(id) - 1 - this.width].id
          const newSquare = this.squares[newId]
          this.click(newSquare.id)
        }
        if (id < 98 && !isRightEdge) {
          const newId = this.squares[parseInt(id) + 1].id
          const newSquare = this.squares[newId]
          this.click(newSquare.id)
        }
        if (id < 90 && !isLeftEdge) {
          const newId = this.squares[parseInt(id) - 1 + this.width].id
          const newSquare = this.squares[newId]
          this.click(newSquare.id)
        }
        if (id < 88 && !isRightEdge) {
          const newId = this.squares[parseInt(id) + 1 + this.width].id
          const newSquare = this.squares[newId]
          this.click(newSquare.id)
        }
        if (id < 89) {
          const newId = this.squares[parseInt(id) + this.width].id
          const newSquare = this.squares[newId]
          this.click(newSquare.id)
        }
      }, 10)
    },

    // On left click -> check the square
    click: function (id) {
      if (this.isGameOver) return
      if (this.squares[parseInt(id)].flag === 'flag' || this.squares[parseInt(id)].checked === 'checked') return

      // If we find a bomb -> end game
      if (this.squares[parseInt(id)].class === 'bomb') {
        this.gameOver(id)
      } else {

        // Show bombs around position
        if (this.squares[parseInt(id)].total !== 0) {
          this.squares[parseInt(id)].checked = 'checked'
          this.squares[parseInt(id)].text = this.squares[parseInt(id)].total
          return
        }

        // Check recursively squares around
        this.checkSquare(this.squares[parseInt(id)].id)
      }
      this.squares[parseInt(id)].checked = 'checked'
    },

    // On event right click -> drop a flag on the square
    addFlag: function (id) {
      if (this.isGameOver === true) return
      if (this.squares[parseInt(id)].checked !== 'checked' && this.flags < this.bombAmount) {
        if (this.squares[parseInt(id)].flag !== 'flag') {
          this.squares[parseInt(id)].flag = 'flag'
          this.squares[parseInt(id)].text = '🚩'
          this.flags++
          this.checkForWin()
        } else {
          this.squares[parseInt(id)].flag = ''
          this.squares[parseInt(id)].text = ''
          this.flags--
        }
      }
    },

    // On click bomb, check gameover value
    gameOver: function () {
      this.isGameOver = true
      for (let square of this.squares) {
        if (square.class === 'bomb') {
          square.text = "💣"
        }
      }
      alert('Game Over')
    },

    // Check flags and bombs on each flag dropped, and check if the game is win
    checkForWin: function () {
      let matches = 0

      for (let i = 0; i < this.squares.length; i++) {
        if (this.squares[i].flag === 'flag' && this.squares[i].class === 'bomb') {
          matches++
        }
        if (matches === this.bombAmount) {
          alert('WIN !!!')
          this.isGameOver = true
        }
      }
    }
  },
  mounted() {
    this.createGrid()
  }
}
</script>

<style scoped>
#grid {
  box-sizing: content-box;
  width: 400px;
  height: 400px;
  display: flex;
  flex-wrap: wrap;
  background-color: grey;
  border: 10px solid #D6D0B7;
}

#grid div {
  box-sizing: border-box;
  width: 40px;
  height: 40px;
  background-color: #D6D0B7;
  border-top: 5px inset #F8F0E5;
  border-left: 5px inset #F8F0E5;
  border-right: 5px inset #B5B0A2;
  border-bottom: 5px inset #B5B0A2;
}

#grid div.checked {
  background: #CAC2B0;
  border: 3px solid #928E80;
}
</style>