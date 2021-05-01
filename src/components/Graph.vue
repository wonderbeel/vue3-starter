<template>
  <div
    ref="graph"
    data-test="billboard-graph"
  />
</template>

<script lang="ts">
import { defineComponent, PropType, ref, computed, watch, nextTick } from 'vue';
import bb, { areaSpline } from 'billboard.js';

interface datasetItem {
  date: string,
  performance: number
}

export default defineComponent({
  props: {
    dataset: {
      type: Object as PropType<datasetItem[]>,
      required: true
    },
  },
  setup(props) {
    console.log(props)
    const graph = ref()
    const graphItem = ref()
    const graphData = computed(() => {
      return {
        json: props.dataset,
        keys: {
          x: 'date',
          value: ['performance'],
        },
        type: areaSpline(),
      };
    })

    const drawGraph = () => {
      graphItem.value = bb.generate({
        data: graphData.value,
        axis: {
          x: {
            show: true,
            type: 'timeseries',
          },
        },
        bindto: graph.value,
      });
    }

    watch(graphData, () => {
      graphItem.value.load(graphData.value)
    })
    nextTick(drawGraph)

    return {
      graph,
      graphItem
    }
  },
});
</script>
