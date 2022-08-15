<template>
  <div class="home">
    <!-- <SfCheckbox
      name="shipping"
      label="I want to create an account"
      hintMessage="Required."
      :required="false"
      infoMessage=""
      errorMessage=""
      valid
      :disabled="false"
      :selected="true"
    /> -->

    <div style="width: 50%; margin: 0 auto;">
      <draggable-two :items="items"></draggable-two>
    </div>

    ...........................................................

    <!-- <draggable :items="items">
    </draggable>

    
    ..../////////////////........................./////////////////////////...........................////////////////
    -->
    
    <dragger>
      <h1>item.title</h1>      
    </dragger>
    

  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import { SfCheckbox } from "@storefront-ui/vue";
import draggable from './draggable.vue'
import draggableTwo from './draggable-two.vue'
import dragger from './dragger.vue'

export default {
  name: 'Home',
  components: {
    HelloWorld,
    SfCheckbox,
    draggable,
    draggableTwo,
    dragger,
  },
  data() {
    return {
      items: [
        {
          uuid: 0,
          title: 'Item A',
          list: 1,
          order: 0,
        },
        {
          uuid: 1,
          title: 'Item B',
          list: 2,
          order: 2,
        },
        {
          uuid: 2,
          title: 'Item C',
          list: 3,
          order: 1,
        },
      ],
    }
  }
}
</script>

<style scoped>

</style>
