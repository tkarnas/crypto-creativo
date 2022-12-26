<template>
  <div>
    <h1>{{ msg }}</h1>
    <button @click="getDataAndExport">Generate more bitcoins</button>
  </div>
</template>

<script>
import axios from "axios";

const API_URL =
  "https://api.coingecko.com/api/v3/coins/markets?" +
  "vs_currency=USD" +
  "&order=volume_desc" +
  "&per_page=55" +
  "&page=1" +
  "&sparkline=false" +
  "&price_change_percentage=24h";

export default {
  props: {
    msg: String,
  },
  name: "HelloWorld",
  data() {
    return {
      data: [],
    };
  },
  methods: {
    async getDataAndExport() {
      try {
        const response = await axios.get(API_URL);
        const data = response.data;

        this.data = data.filter(
          (crypto) =>
            crypto.id !== "tether" &&
            crypto.id !== "usd-coin" &&
            crypto.id !== "binance-usd"
        );

        const text = this.data
          .map((crypto) => "BINANCE:" + crypto.symbol.toUpperCase() + "USDT\n")
          .join("");
        const blob = new Blob([text], { type: "text/plain" });
        const link = document.createElement("a");
        link.href = window.URL.createObjectURL(blob);
        link.download = "binance.txt";
        link.click();
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>