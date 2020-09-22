<template>
  <div id="grid">
    <div v-for="square in squares" :key="square.id" :class="square.class"></div>
  </div>
</template>

<script>
export default {
  name: "Grid",
  data() {
    return {
      width: 10,
      gridSize: this.width * this.width,
      squares: [],
      bombAmount: 20
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
          class: shuffleArray[i]
        }
        this.squares.push(square);
      }

      // Ajout des numeros
      for (let i = 0; i < this.squares.length; i++) {
        const isLeftEdge = (i % this.width === 0);
        const isRightEdge = (i % this.width === this.width - 1);
        let total = 0;

        if (this.squares[i].class == 'valid') {
          if (i > 0 && !isLeftEdge && this.squares[i - 1].class == 'bomb') {
            total++;
          }
          if (i > 9 && !isRightEdge && this.squares[i + 1 - this.width].class == 'bomb') {
            total++;
          }
          if (i > 10 && this.squares[i - this.width].class == 'bomb') {
            total++;
          }
          if (i > 11 && !isLeftEdge && this.squares[i - 1 - this.width].class == 'bomb') {
            total++;
          }
          if (i < 98 && !isRightEdge && this.squares[i + 1].class == 'bomb') {
            total++;
          }
          if (i < 90 && !isLeftEdge && this.squares[i - 1 + this.width].class == 'bomb') {
            total++;
          }
          if (i < 88 && !isRightEdge && this.squares[i + 1 + this.width].class == 'bomb') {
            total++;
          }
          if (i < 89 && this.squares[i + this.width].class == 'bomb') {
            total++;
          }
          this.squares[i].data = total;
          console.log(this.squares[i]);
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

.bomb {
  background-color: orange;
}
</style>