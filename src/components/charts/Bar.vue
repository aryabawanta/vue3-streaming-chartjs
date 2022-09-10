<template>
  <div class="container">
    <Bar
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
import { Bar } from "vue-chartjs";
import {
  Chart as ChartJS,
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale,
  PluginOptionsByType,
} from "chart.js";

ChartJS.register(
  Title,
  Tooltip,
  Legend,
  BarElement,
  CategoryScale,
  LinearScale
);

interface Props {
  chartId?: String;
  width?: Number;
  height?: Number;
  cssClasses?: String;
  plugins?: PluginOptionsByType<"bar">;
  styles?: Partial<CSSStyleDeclaration>;
}

const {
  chartId = "bar-chart", 
  width = 400, 
  height = 400, 
  cssClasses = "", 
  styles = {}, 
  plugins = {}
} = defineProps<Props>();

const chartData = {
  labels: ["January", "February", "March"],
  datasets: [{ data: [40, 20, 12] }],
}

const chartOptions = { responsive: true };
</script>

<style>
.container {
  padding: 20px;
  background: white;
}
</style>
