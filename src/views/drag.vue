<template>
  <div id="app">
    <p>Drag the p element back and forth between the two rectangles:</p>
    <div
      class="droptarget"
      v-on:drop="drop($event)"
      v-on:dragover="allowDrop($event)"
    >
      <p
      v-on:dragstart="dragStart($event)"
        v-on:drag="dragging($event)"
        draggable="true"
        id="dragtarget"
      >
        Drag me!
      </p>
    </div>

    <div
      class="droptarget"
      v-on:drop="drop"
      v-on:dragover="allowDrop($event)"
    ></div>

    <p style="clear:both;">
      <strong>Note:</strong> drag events are not supported in Internet
      Explorer 8 and earlier versions or Safari 5.1 and earlier versions.
    </p>

    <p id="demo"></p>
  </div>
</template>

<script>
  export default {
    data() {
      return {

      }
    },
    methods: {
      dragStart(event)  {
        console.log('dragStart event', event)
        event.dataTransfer.setData("Text", event.target.id);
      },
      dragging(event) {
        // console.log('dragging', event)
        document.getElementById("demo").innerHTML = "The p element is being dragged";
      },
      allowDrop(event) {
        console.log('allowDrop', event)
        event.preventDefault();
      },
      drop(event) {
        console.log('drop', event)
        event.preventDefault();
        var data = event.dataTransfer.getData("Text");
        event.target.appendChild(document.getElementById(data));
        document.getElementById("demo").innerHTML = "The p element was dropped";
      }
    }
  }
</script>

<style scoped>
.droptarget {
  float: left; 
  width: 100px; 
  height: 35px;
  margin: 15px;
  padding: 10px;
  border: 1px solid #aaaaaa;
}
</style>