<template>
  <div
      ref="square"
      @click="$emit('click', id)"
      @contextmenu.prevent="$emit('contextMenu', id)"
      :class="[classItem, checked, flag]">
    {{ text }}
  </div>
</template>

<script>
export default {
  name: "Square",
  props: ['id', 'classItem', 'total', 'checked', 'flag', 'text']
}
</script>

<style scoped>
.bomb {
  background-color: orange;
}
</style>