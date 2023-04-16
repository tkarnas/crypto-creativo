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
            (crypto) => "BINANCE:" + crypto.symbol.toUpperCase() + "USDT.P\n"
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
  background-color: #8e24aa;
  border: none;
  color: whitesmoke;
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
  animation: pulse 2s ease-in-out infinite;
}

@keyframes pulse {
  0% {
    transform: scale(1);
  }
  50% {
    transform: scale(1.1);
  }
  100% {
    transform: scale(1);
  }
}

button:hover {
  transform: scale(1.1);
  background-color: #ff7300;
}

img {
  width: 540px;
  height: 540px;
  display: block;
  margin: 0 auto;
}

h1 {
  font-size: 3em;
  text-align: center;
  color: #ff7300;
  background: #ff7300;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  letter-spacing: 0.1em;
  margin-top: 2em;
  animation: animateText 3s ease-in-out infinite;
}

@keyframes animateText {
  0% {
    transform: rotate(0deg);
  }
  25% {
    transform: rotate(5deg);
  }
  50% {
    transform: rotate(-5deg);
  }
  75% {
    transform: rotate(3deg);
  }
  100% {
    transform: rotate(0deg);
  }
}
@media only screen and (max-width: 700px) {
  button {
    font-size: 14px;
    padding: 10px 20px;
  }

  img {
    width: 80%;
    height: 80%;
  }

  h1 {
    font-size: 2em;
  }
}
</style>