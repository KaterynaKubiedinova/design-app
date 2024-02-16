<template>
  <div 
    class="draggable"
    :style="{ top: position.top + 'px', left: position.left + 'px' }"
    @mousedown="startDragging"
    @mousemove="dragging"
    @mouseup="stopDragging"
    @mouseleave="stopDragging"
  >
  <v-textarea
      color="purple"
      style="width: 100%;"
      no-resize
      :overflow="hidden"
      variant="plain"
      placeholder="Your text"
      hide-details>{{ text.text }}
    </v-textarea>
  </div>
</template>

<script>
export default {
  name: 'DraggableElement',
  props: {
    text: Object
  },
  data() {
    return {
      isDragging: false,
      startPosition: { x: 0, y: 0 },
      currentPosition: { x: 60, y: 10 },
      position: { top: 10, left: 60 },
      
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
        this.position.left = this.currentPosition.x + deltaX;
        this.position.top = this.currentPosition.y + deltaY;
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
.draggable {
  position: absolute;
  cursor: move;
  border: 1px solid black;
  background-color: rgb(219, 91, 208);
  padding: 7px;
  width: auto;
  height: auto;
  min-height: 100px;
  min-width: 120px;
}
</style>
