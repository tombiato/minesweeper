<template>
  <div ref="square" @click="$emit('click', id)" :class="[classItem, checked, flag]">{{ text }}</div>
</template>

<script>
export default {
  name: "Square",
  props: ['id', 'classItem', 'total', 'checked', 'flag', 'text'],
  data() {
    return {
    }
  },
  methods: {
    click: function () {
      if (this.isGameOver) return
      if (this.flag === 'flag' || this.checked === 'checked') return
      if (this.classItem === 'bomb') {
        console.log('boom')
      } else {
        if (this.total !== 0) {
          this.text = this.total
          this.checked = 'checked'

          return
        }
        this.$emit('checkSquare', this.$ref.click(), this.id)
      }
      this.checked = 'checked'
    }
  }
}
</script>

<style scoped>
.bomb {
  background-color: orange;
}

.checked {
  background: red;
}
</style>