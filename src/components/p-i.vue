<template>
  <div 
    class="container"
    ref="container"
    @mousedown="startDragging"
    @mousemove="dragging"
    @mouseup="stopDragging"
  >
    <div 
      class="draggable"
      ref="draggable"
      :style="{ top: position.top + 'px', left: position.left + 'px' }"
    >
      <v-text-field>d</v-text-field>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isDragging: false,
      startPosition: { x: 0, y: 0 },
      currentPosition: { x: 0, y: 0 },
      position: { top: 0, left: 0 }
    };
  },
  methods: {
    startDragging(event) {
      this.isDragging = true;
      this.startPosition.x = event.clientX;
      this.startPosition.y = event.clientY;
    },
    dragging(event) {
      if (this.isDragging) {
        const deltaX = event.clientX - this.startPosition.x;
        const deltaY = event.clientY - this.startPosition.y;

        const containerRect = this.$refs.container.getBoundingClientRect();
        const draggableRect = this.$refs.draggable.getBoundingClientRect();

        const maxX = containerRect.width - draggableRect.width;
        const maxY = containerRect.height - draggableRect.height;

        let newLeft = this.currentPosition.x + deltaX;
        let newTop = this.currentPosition.y + deltaY;

        newLeft = Math.max(0, Math.min(maxX, newLeft));
        newTop = Math.max(0, Math.min(maxY, newTop));

        this.position.left = newLeft;
        this.position.top = newTop;
      }
    },
    stopDragging() {
      if (this.isDragging) {
        this.isDragging = false;
        this.currentPosition.x = this.position.left;
        this.currentPosition.y = this.position.top;
      }
    }
  }
};
</script>

<style>
.container {
  position: relative;
  width: 300px;
  height: 200px;
  border: 1px solid black;
  padding: 10px;
}

.draggable {
  position: absolute;
  cursor: pointer;
  border: 1px solid black;
  padding: 10px;
}
</style>
