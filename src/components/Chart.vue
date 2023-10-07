<script setup>
import { onUnmounted, onMounted, ref, watch } from "vue";
import { createChart } from "lightweight-charts";

const props = defineProps({
  data: {
    type: Array,
    required: true,
  },
  autosize: {
    type: Boolean,
    default: true,
  },
});

let chart;
const chartContainer = ref({
  layout: { textColor: "black", background: { type: "solid", color: "white" } },
});

const resizeHandler = () => {
  if (!chart || !chartContainer.value) return;
  const dimensions = chartContainer.value.getBoundingClientRect();
  chart.resize(dimensions.width, dimensions.height);
};

onMounted(() => {
  // Create the Lightweight Charts Instance using the container ref.
  chart = createChart(chartContainer.value);

  const candlestickSeries = chart.addCandlestickSeries({
    upColor: "#26a69a",
    downColor: "#ef5350",
    borderVisible: false,
    wickUpColor: "#26a69a",
    wickDownColor: "#ef5350",
  });

  candlestickSeries.setData(props.data);

  chart.timeScale().fitContent();

  if (props.autosize) {
    window.addEventListener("resize", resizeHandler);
  }
});

onUnmounted(() => {
  if (chart) {
    chart.remove();
    chart = null;
  }
});

watch(
  () => props.autosize,
  (enabled) => {
    if (!enabled) {
      window.removeEventListener("resize", resizeHandler);
      return;
    }
    window.addEventListener("resize", resizeHandler);
  }
);
</script>
<template>
  <div class="lw-chart" ref="chartContainer"></div>
</template>
<style scoped>
.lw-chart {
  height: 80%;
  width: 80%;
}

@media all and (max-width: 800px) {
  .lw-chart {
    width: 100%;
  }
}

@media all and (min-width: 800px) {
  .lw-chart {
    width: 80%;
  }
}

@media all and (min-width: 1200px) {
  .lw-chart {
    width: 80%;
  }
}
</style>
