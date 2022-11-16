<script setup lang="ts">
import { ref, onMounted } from 'vue';
import { dia, ui, shapes, g } from '@clientio/rappid';

const props = withDefaults(defineProps<{
  message: string
}>(), {
  message: 'Hello, world!',
})

const graph: dia.Graph = new dia.Graph({}, { cellNamespace: shapes });
const paper: dia.Paper = new dia.Paper({
  model: graph,
  background: {
    color: '#F8F9FA',
  },
  frozen: true,
  async: true,
  sorting: dia.Paper.sorting.APPROX,
  cellViewNamespace: shapes
});
const scroller: ui.PaperScroller = new ui.PaperScroller({
  paper,
  autoResizePaper: false,
  cursor: 'grab'
});

scroller.render();

const rect = new shapes.standard.Rectangle({
  position: { x: 100, y: 100 },
  size: { width: 100, height: 50 },
  attrs: {
    label: {
      text: props.message
    }
  }
})

graph.addCell(rect);

const canvas = ref<HTMLDivElement>()



onMounted(() => {
  if (!canvas.value) return;
  (canvas.value as HTMLDivElement).appendChild(scroller.el);
  console.log('mounted');
  //scroller.center();
  paper.unfreeze();
})

</script>

<template>
  <div class="canvas" ref="canvas" />
</template>


<style lang="scss" scoped>
@import "@clientio/rappid/dist/rappid.css";

body {
  height: 100vh;
  box-sizing: border-box;
  margin: 0;

  .canvas {
    width: 100%;
    height: 100%;

    .joint-paper {
      border: 1px solid #A0A0A0;
    }
  }
}
</style>