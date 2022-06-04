<template>
  <trading-vue
    :data="chart"
    :width="this.width"
    :height="this.height"
    :index_based="this.index_based"
    ref="tradingVue"
  ></trading-vue>
</template>

<script>
import { TradingVue, DataCube } from "trading-vue-js";

export default {
  name: "app",
  components: { TradingVue },
  data() {
    return {
      chart: {},
      width: window.innerWidth,
      height: window.innerHeight,
      index_based: false,
    };
  },
  mounted() {
    console.log("TradingVue: mounted");
    window.addEventListener("resize", this.onResize);
    this.onResize();

    this.load_chunk();

    this.chart = new DataCube(
      {
        ohlcv: [
          [1643535000, 1000, 1000, 1000, 1000, 100],
          [1643535060, 1000, 1000, 1000, 1000, 100],
          [1643535120, 1000, 1000, 1000, 1000, 100],
        ],
        onchart: [
          {
            type: "EMAx6",
            name: "Multiple EMA",
            data: [],
          },
        ],
        offchart: [
          {
            type: "BuySellBalance",
            name: "Buy/Sell Balance, $lookback",
            data: [],
            settings: {},
          },
        ],
        datasets: [
          {
            type: "Trades",
            id: "binance-btcusdt",
            data: [],
          },
        ],
      },
      { aggregation: 100 }
    );

    this.$refs.tradingVue.resetChart();

    setTimeout(() => {
      this.chart.update({
        t: 1643535180, // Exchange time (optional)
        price: parseFloat(1000), // Trade price
        volume: parseFloat(100), // Trade amount
        "datasets.binance-btcusdt": [
          // Update dataset
          1643535180,
          0, // Sell or Buy
          parseFloat(100),
          parseFloat(1000),
        ],
        // ... other onchart/offchart updates
      });
    }, 0);
  },
  methods: {
    onResize() {
      this.width = window.innerWidth;
      this.height = window.innerHeight - 3;
    },
    async load_chunk() {
      const url =
        "https://us-central1-clone-inago.cloudfunctions.net/helloWorld?symbol=BTCUSD&interval=1&limit=2&from=1581231260";
      const request = require("request-promise");
      var options = {
        url: url,
        method: "GET",
      };
      var aa = await request(options);
      console.log(aa);
      // const res = await fetch(url, { mode: "cors" }).then((r) => r.json());
      // return this.format(this.parse_binance(res));
    },
    parse_binance(data) {
      if (!Array.isArray(data)) return [];
      return data.map((x) => {
        for (var i = 0; i < x.length; i++) {
          x[i] = parseFloat(x[i]);
        }
        return x.slice(0, 6);
      });
    },
    format(data) {
      return {
        "chart.data": data,
      };
    },
  },
};
</script>

<style>
</style>