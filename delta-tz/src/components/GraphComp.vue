<template>
    <div class="graph">
            <Line
    id="my-chart-id"
    :options="chartOptions"
    :data="chartData"
  />
    </div>
  </template>
  <script>
import {
  Chart as ChartJS,
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
} from 'chart.js'
import { Line } from 'vue-chartjs'

ChartJS.register(
  CategoryScale,
  LinearScale,
  PointElement,
  LineElement,
  Title,
  Tooltip,
  Legend
)
  
  export default {
    methods: {

    },
    name: 'BarChart',
    // eslint-disable-next-line vue/no-reserved-component-names
    components: { Line },
    props: ['chartDataP'],
    data() {
      return {
      }
    },
    computed: {
      chartData() {
         console.log( this.chartDataP.datasets)
         return this.chartDataP
    },
      chartOptions() { return {
        responsive: true,
            maintainAspectRatio: false
      } }
    },
  }
  </script>
  
  <style>
    .graph {
        position: absolute;
      width: 100%;
      background-color: #FFF;
      padding:25px 0px;
    }
  </style>