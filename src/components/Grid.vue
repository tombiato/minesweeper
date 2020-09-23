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
            @click="click"/>
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
      isGameOver: false
    }
  },
  methods: {
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
          const newId = this.squares[parseInt(id) - this.width].id
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
    click: function (id) {
      if (this.isGameOver) return
      if (this.squares[parseInt(id)].flag === 'flag' || this.squares[parseInt(id)].checked === 'checked') return
      if (this.squares[parseInt(id)].class === 'bomb') {
        this.isGameOver = true
        console.log('boom')
      } else {
        if (this.squares[parseInt(id)].total !== 0) {
          this.checked = 'checked'
          this.squares[parseInt(id)].text = this.squares[parseInt(id)].total
          return
        }
        this.checkSquare(this.squares[parseInt(id)].id)
      }
      this.checked = 'checked'
    }
  },
  mounted() {
    this.createGrid()
  }
}
</script>

<style scoped>
#grid {
  width: 400px;
  height: 400px;
  display: flex;
  flex-wrap: wrap;
  background-color: grey;
}

#grid div {
  width: 40px;
  height: 40px;
}
</style>