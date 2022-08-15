<template>
  <div>
    <div>
      <div @dragleave="mouseUpHandler($event)">
        <div draggable="true"
          @dragover="mouseMoveHandler($event)"
          @dragstart="mouseDownHandler($event)"
          v-for="(item, index) in getItems(items)"
          :key="item.id"
          class="draggable"
        >{{item.title}} --- index {{index}} - item index {{item.index}}</div>
        <!-- <div draggable="true" 
          @dragover="mouseMoveHandler($event)"
          @dragstart="mouseDownHandler($event)" 
          class="draggable"
        >B</div>
        <div draggable="true" 
          @dragover="mouseMoveHandler($event)"
          @dragstart="mouseDownHandler($event)" 
          class="draggable"
        >C</div>
        <div draggable="true" 
          @dragover="mouseMoveHandler($event)"
          @dragstart="mouseDownHandler($event)" 
          class="draggable"
        >D</div>
        <div draggable="true" 
          @dragover="mouseMoveHandler($event)"
          @dragstart="mouseDownHandler($event)" 
          class="draggable"
        >E</div> -->
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    items: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
      x: null,
      y: null,
      draggingEle: null,
      placeholder: null,
      isDraggingStarted: false,
    }
  },
  created() {
    // this.$el.addEventListener('DOMContentLoaded', function () {
      // const list = document.getElementById('list');
    // });

    // Query all items
    // [].slice.call(list.querySelectorAll('.draggable')).forEach(function (item) {
    //     item.addEventListener('mousedown', this.mouseDownHandler());
    // });
  },
  methods: {
    getItems(items) {
      return items.sort(function(a, b){ return a.index - b.index });
    },
    swap(nodeA, nodeB) {
      const parentA = nodeA.parentNode;
      const siblingA = nodeA.nextSibling === nodeB ? nodeA : nodeA.nextSibling;

      // Move `nodeA` to before the `nodeB`
      nodeB.parentNode.insertBefore(nodeA, nodeB);

      // Move `nodeB` to before the sibling of `nodeA`
      parentA.insertBefore(nodeB, siblingA);
    },
    // Check if `nodeA` is above `nodeB`
    isAbove(nodeA, nodeB) {
      // Get the bounding rectangle of nodes
      const rectA = nodeA.getBoundingClientRect();
      const rectB = nodeB.getBoundingClientRect();

      return rectA.top + rectA.height / 2 < rectB.top + rectB.height / 2;
    },
    mouseDownHandler(e) {
      this.draggingEle = e.target;

      // Calculate the mouse position
      const rect = this.draggingEle.getBoundingClientRect();
      this.x = e.pageX - rect.left;
      this.y = e.pageY - rect.top;

      // Attach the listeners to `document`
      // document.addEventListener('mousemove', this.mouseMoveHandler(e));
      // document.addEventListener('mouseup', this.mouseUpHandler(e));
    },
    mouseMoveHandler(e) {
      const draggingRect = this.draggingEle.getBoundingClientRect();

      if (!this.isDraggingStarted) {
          this.isDraggingStarted = true;

          // Let the placeholder take the height of dragging element
          // So the next element won't move up
          this.placeholder = document.createElement('div');
          this.placeholder.classList.add('placeholder');
          this.draggingEle.parentNode.insertBefore(this.placeholder, this.draggingEle.nextSibling);
          this.placeholder.style.height = `${draggingRect.height}px`;
      }

      // Set position for dragging element
      this.draggingEle.style.position = 'absolute';
      this.draggingEle.style.top = `${e.pageY - this.y}px`;
      this.draggingEle.style.left = `${e.pageX - this.x}px`;

      // The current order
      // prevEle
      // draggingEle
      // placeholder
      // nextEle
      const prevEle = this.draggingEle.previousElementSibling;
      const nextEle = this.placeholder.nextElementSibling;

      // The dragging element is above the previous element
      // User moves the dragging element to the top
      if (prevEle && this.isAbove(this.draggingEle, prevEle)) {
          // The current order    -> The new order
          // prevEle              -> placeholder
          // draggingEle          -> draggingEle
          // placeholder          -> prevEle
          this.swap(this.placeholder, this.draggingEle);
          this.swap(this.placeholder, prevEle);
          return;
      }

      // The dragging element is below the next element
      // User moves the dragging element to the bottom
      if (nextEle && this.isAbove(nextEle, this.draggingEle)) {
          // The current order    -> The new order
          // draggingEle          -> nextEle
          // placeholder          -> placeholder
          // nextEle              -> draggingEle
          this.swap(nextEle, this.placeholder);
          this.swap(nextEle, this.draggingEle);
      }
    },
    mouseUpHandler(e) {
      // Remove the placeholder
      this.placeholder && this.placeholder.parentNode.removeChild(this.placeholder);

      this.draggingEle.style.removeProperty('top');
      this.draggingEle.style.removeProperty('left');
      this.draggingEle.style.removeProperty('position');

      this.x = null;
      this.y = null;
      this.draggingEle = null;
      this.isDraggingStarted = false;
      this.placeholder = null;

      // Remove the handlers of `mousemove` and `mouseup`
      // document.removeEventListener('mousemove', this.mouseMoveHandler(e));
      // document.removeEventListener('mouseup', this.mouseUpHandler(e));
    },
  }
}
</script>

<style scoped>
.draggable {
  cursor: move;
  margin-bottom: 1rem;
  user-select: none;

  align-items: center;
  display: flex;
  justify-content: center;

  height: 4rem;
  width: 16rem;
  background-color: #eee;
  

  border: 1px solid #333;
}
.placeholder {
  background-color: black;
  border: 2px dashed green;
  margin-bottom: 5rem;
}
</style>