<template>
  <div>
    <div class="list">
      <div draggable="true"
        @dragstart="draggingStarted($event)"
        @dragend="draggingEnded($event)"
        @dragover="draggingOver($event)"
        class="draggable"
      >
        <slot />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      
    }
  },
  methods: {
    draggingStarted(e) {
      const draggingEle = e.target;
      draggingEle.classList.add('dragging');
    },
    draggingEnded(e) {
      const draggingEle = e.target;
      draggingEle.classList.remove('dragging');
    },
    draggingOver(e) {
      const container = document.querySelectorAll('.list')
      const afterElement = this.getDragAfterElement(container[0], e.clientY)
      const draggable = document.querySelector('.dragging')
      if (afterElement == null) {
        container[0].appendChild(draggable)
      } else {
        container[0].insertBefore(draggable, afterElement)
      }
      const elements = document.querySelectorAll('.draggable')
      console.log('elements', elements)
      elements.forEach((item, index) => {
        let foundItem = this.items.find((i) => i.id == item.id)
        foundItem.index = index;
      });
    },
    getDragAfterElement(container, y) {      
      const notDraggingElements = [...container.querySelectorAll('.draggable:not(.dragging)')]

      return notDraggingElements.reduce((closest, child) => {
        const box = child.getBoundingClientRect();
        const offset = y - box.top - box.height / 2
        if (offset < 0 && offset > closest.offset) {
          return { offset: offset, element: child }
        } else {
          return closest
        }
      }, { offset: Number.NEGATIVE_INFINITY }).element
    }
  }
}
</script>

<style scoped>
body {
  margin: 0;
}

.container {
  background-color: #333;
  padding: 1rem;
  margin-top: 1rem;
}

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

.draggable.dragging {
  opacity: .5;
  background-color: #edf2f7;
  border: 2px dashed #cbd5e0;
  margin-bottom: 1rem;
}
</style>