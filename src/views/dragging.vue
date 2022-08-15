<template>
  <div>
    <!-- <div class="main">
      <div class="container">
        <div class="draggable" draggable="true">1</div>
        <div class="draggable" draggable="true">2</div>
      </div>
      <div class="container">
        <div class="draggable" draggable="true">3</div>
        <div class="draggable" draggable="true">4</div>
      </div>
    </div> -->

    <div>
      <div class="drop-zone" @drop="onDrop($event)" @dragover.prevent @dragenter.prevent>
        <div
          class="drag-el"
          v-for="item in getItems"
          :key="item.title"
          draggable
          @dragstart="startDrag($event, item)"
        >
          {{ item.title }} {{ item.index }}
        </div>
      </div>
      <!-- <div class="drop-zone" @drop="onDrop($event, 2)" @dragover.prevent @dragenter.prevent>
        <div
          class="drag-el"
          v-for="item in listTwo"
          :key="item.title"
          draggable
          @dragstart="startDrag($event, item)"
        >
          {{ item.title }}
        </div>
      </div>
      <div class="drop-zone" @drop="onDrop($event, 3)" @dragover.prevent @dragenter.prevent>
        <div
          class="drag-el"
          v-for="item in listThree"
          :key="item.title"
          draggable
          @dragstart="startDrag($event, item)"
        >
          {{ item.title }}
        </div>
      </div> -->
    </div>

  </div>
</template>

<script>
export default {
  data() {
    return {
      items: [
        {
          id: 0,
          title: 'Item A',
          list: 1,
          index: 0,
        },
        {
          id: 1,
          title: 'Item B',
          list: 2,
          index: 2,
        },
        {
          id: 2,
          title: 'Item C',
          list: 3,
          index: 1,
        },
      ],
    }
  },
  computed: {
    // listOne() {
    //   return this.items.filter((item) => item.list === 1)
    // },
    // listTwo() {
    //   return this.items.filter((item) => item.list === 2)
    // },
    // listThree() {
    //   return this.items.filter((item) => item.list === 3)
    // }
    getItems() {
      return this.items.sort(function(a, b){ return a.index - b.index; });
    }
  },
  methods: {
    startDrag(evt, item) {
      console.log('startDrag', 'evt', evt, 'item', item)
      evt.dataTransfer.dropEffect = 'move'
      evt.dataTransfer.effectAllowed = 'move'
      evt.dataTransfer.setData('itemID', item.id)
    },
    onDrop(evt) {
      console.log('ondrop', 'evt', evt,)
      const itemID = evt.dataTransfer.getData('itemID')
      const item = this.items.find((item) => item.id == itemID)
      // item.list = list
      // this.makeIndex();
    },
    // makeIndex() {
    //   this.items.forEach((item, index) => {
    //     item.index = index;
    //   })
    // }
  },
}
</script>

<style scoped>
/* body {
  margin: 0;
}
.container {
  background-color: #333;
  padding: 1rem;
  margin: 1rem;
}
.draggable {
  padding: 1rem;
  background-color: white;
  border: 1px solid black;
}
.main {
  width: 50%;
  margin: 0 auto;
} */
.drop-zone {
  background-color: #eee;
  margin-bottom: 10px;
  padding: 10px;
}
.drag-el {
  background-color: #fff;
  margin-bottom: 10px;
  padding: 5px;
}
</style>