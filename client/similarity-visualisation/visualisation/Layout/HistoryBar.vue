<template>
  <v-content>
    <p>Visited nodes</p>
    <template v-for="(c, index) in visitedNodes">
      <v-btn
        v-bind:content="`
          label: ${c.label}</br>
          id: ${c.id}
        `"
        v-tippy='{interactive : true, animateFill: false, placement:"left", animation:"shift-toward", delay:10, arrow : true}'
        v-bind:key="index"
        class="btn-help ma-2" @click="click(index)"
        outlined
        large
        fab
        color="black"
      >
        {{ c.label.length > 5 ? c.label.substring(0, 5) + ".." : c.label }}
      </v-btn>
    </template>
  </v-content>
</template>

<script lang="ts">
import Vue from 'vue'
import { Getters, Actions, STORE_NAME } from '../Visualisation.store'
import { mapGetters, mapActions, Store } from 'vuex'
import { VisitedNode } from '../../models/VisitedNode'

export default Vue.extend({
  name: 'HistoryBar',

  props: ['activeView'],
  data: () => ({
  }),
  computed: {
    ...mapGetters(STORE_NAME, {
      visitedNodes: Getters.GET_VISITED_NODES
    })
  },
  methods: {
    ...mapActions(STORE_NAME, {
      updatePath: Actions.UPDATE_PATH,
      createHierarchyForCircles: Actions.CREATE_HIERARCHY_FOR_CIRCLES,
      updateCircleCanvas: Actions.UPDATE_CIRCLE_CANVAS,
      createHierarchyForTree: Actions.CREATE_HIERARCHY_FOR_TREE,
      updateTreeCanvas: Actions.UPDATE_TREE_CANVAS
    }),
    click: function (data: VisitedNode) {
      this.updatePath(data)
      if (this.activeView === 1) {
          this.createHierarchyForCircles()
          this.updateCircleCanvas()
      }
      if (this.activeView === 2) {
        this.createHierarchyForTree()
        this.updateTreeCanvas()
      }
    }
  }
})
</script>

<style>
.btn-help {
  text-transform: none;
}
.circle {
  cursor: pointer;
}
.labels {
  cursor: pointer;
  text-anchor: middle;
  pointer-events: none;
}
</style>
