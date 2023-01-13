<template>
  <div>
    <img alt="Vue logo" src="../assets/btc.jpeg" />
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
  "&per_page=100" +
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
          .map(
            (crypto) => "BINANCE:" + crypto.symbol.toUpperCase() + "USDTPERP\n"
          )
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

<style scoped>
button {
  background-color: #4caf50; /* Green */
  border: none;
  color: white;
  padding: 15px 32px;
  text-align: center;
  text-decoration: none;
  display: inline-block;
  font-size: 16px;
  margin: 4px 2px;
  cursor: pointer;
  border-radius: 10px;
  box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
  transition: all 0.2s ease-in-out;
}

button:hover {
  transform: scale(1.1);
  background-color: #3e8e41;
}

img {
  width: 540px;
  height: 540px;
  display: block;
  margin: 0 auto;
}
</style>