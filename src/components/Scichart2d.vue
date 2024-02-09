<template>
  <div class="hello">
    <h1>{{ msg }}</h1>
    <div
      id="scichart-root"
      style="width: 600px; height: 400px; margin: auto"
    ></div>
  </div>
</template>

<script>
import axios from "axios";
import { defineComponent, onMounted } from "vue";
import {
  SciChartSurface,
  NumericAxis,
  XyDataSeries,
  FastLineRenderableSeries,
} from "scichart";

export default defineComponent({
  data() {
    return {
      trades: [],
    };
  },
  mounted() {
    this.fetchTrades();
  },
  methods: {
    async fetchTrades() {
      try {
        const response = await axios.get(
          "https://rest.statica.pl/rest/stockquotes/gpw:PLKGHM000017?type=trades&dt_from=2010-01-01&limit=10000",
          {
            headers: {
              Authorization: "Basic " + btoa("frontend2024:test"),
            },
          }
        );

        this.trades = response.data;

        this.initSciChart();
      } catch (error) {
        console.error("Błąd podczas pobierania danych:", error);
      }
    },
    async initSciChart() {
      const { sciChartSurface, wasmContext } = await SciChartSurface.create(
        "scichart-root"
      );

      const xAxis = new NumericAxis(wasmContext);
      const yAxis = new NumericAxis(wasmContext);

      const dataSeries = new XyDataSeries(wasmContext);

      this.trades.forEach((trade) => {
        dataSeries.append(trade.dt, trade.price, trade.amount);
      });

      const series = new FastLineRenderableSeries(wasmContext, { dataSeries });
      sciChartSurface.renderableSeries.add(series);

      sciChartSurface.xAxes.add(xAxis);
      sciChartSurface.yAxes.add(yAxis);
    },
  },
  onMounted() {
    this.fetchTrades();
  },
  name: "scichart2d",
  props: {
    msg: String,
  },
});
</script>
