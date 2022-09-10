<template>
  <div class="container">
    <!-- <div id="streaming-chart" width="500" height="500" /> -->

    <Line
      :chart-options="chartOptions"
      :chart-data="chartData"
      :chart-id="chartId"
      :dataset-id-key="datasetIdKey"
      :plugins="plugins"
      :css-classes="cssClasses"
      :styles="styles"
      :width="width"
      :height="height"
    />
  </div>
</template>

<script lang="ts" setup>
// import { onMounted } from "vue";
import {
  Chart,
  Title,
  Tooltip,
  Legend,
  CategoryScale,
  LinearScale,
  PluginOptionsByType,
  LineElement,
  PointElement,
} from "chart.js";
import { Line } from "vue-chartjs";
import "chartjs-adapter-luxon";
import ChartStreaming from "chartjs-plugin-streaming";

Chart.register(
  ChartStreaming,
  Title,
  Tooltip,
  Legend,
  LineElement,
  LinearScale,
  PointElement,
  CategoryScale
);

Chart.defaults.set("plugins.streaming", {
  duration: 20000,
});

interface Props {
  chartId?: String;
  width?: Number;
  height?: Number;
  cssClasses?: String;
  plugins?: PluginOptionsByType<"line">;
  styles?: Partial<CSSStyleDeclaration>;
}

const {
  chartId = "streaming-chart", 
  width = 400, 
  height = 400, 
  cssClasses = "", 
  styles = {}, 
  plugins = {}
} = defineProps<Props>();

const chartData = {
  datasets: [
    {
      label: 'Dataset 1 (Linear Interpolation)',
      // backgroundColor: Utils.transparentize(Utils.CHART_COLORS.red, 0.5),
      // borderColor: Utils.CHART_COLORS.red,
      borderDash: [8, 4],
      data: []
    },
    {
      label: 'Dataset 2 (Cubic Interpolation)',
      // backgroundColor: Utils.transparentize(Utils.CHART_COLORS.blue, 0.5),
      // borderColor: Utils.CHART_COLORS.blue,
      cubicInterpolationMode: 'monotone',
      data: []
    }
  ]
};

const onRefresh = chart => {
  const now = Date.now();
  chart.data.datasets.forEach(dataset => {
    dataset.data.push({
      x: now,
      y: Math.floor(Math.random() * 100),
    });
  });
};

const chartOptions = {
  scales: {
    x: {
      type: 'realtime',
      realtime: {
        duration: 20000,
        refresh: 1000,
        delay: 2000,
        onRefresh: onRefresh
      },
    },
    y: {
      title: {
        display: true,
        text: 'Value'
      }
    }
  },
  interaction: {
    intersect: false
  }
};



// onMounted(() => {
//   const ctx = document.getElementById("streaming-chart");
//   new Chart(ctx, options);
// });
</script>

<style>
.container {
  padding: 20px;
  background: white;
}
</style>
