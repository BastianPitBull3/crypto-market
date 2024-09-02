<template>
  <div class="container">
    <div class="row">
      <h1>Crypto Market</h1>
      <input
        type="text"
        class="form-control rounded-0 border-0 my-4"
        placeholder="Search Coin"
        @keyup="searchCoin()"
        v-model="textSearch"
      />
      <table class="table table-dark">
        <thead>
          <tr>
            <th v-for="title in titles" :key="title">
              <tr>
                {{
                  title
                }}
              </tr>
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(coin, index) in filteredCoins" :key="coin.id">
            <td class="text-secondary">{{ index + 1 }}</td>
            <td>
              <img
                :src="coin.image"
                style="width: 2rem"
                alt="coin.name"
                class="me-2"
              />
              <span>{{ coin.name }}</span>
              <span class="ms-2 text-uppercase text-secondary">{{
                coin.symbol
              }}</span>
            </td>
            <td>${{ coin.current_price.toLocaleString() }}</td>
            <td
              :class="[
                coin.price_change_percentage_24h > 0
                  ? 'text-success'
                  : 'text-danger',
              ]"
            >
              {{ coin.price_change_percentage_24h }}
            </td>
            <td>${{ coin.total_volume.toLocaleString() }}</td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      coins: [],
      filteredCoins: [],
      titles: ["#", "Coin", "Price", "Price Change", "24h Volume"],
      textSearch: '',
    };
  },
  async mounted() {
    const res = await fetch(
      "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&x_cg_demo_api_key=CG-74xhka15CTacxdELFirEJ1A2"
    );
    const data = await res.json();
    console.log(data);
    this.coins = data;
    this.filteredCoins = data;
  },
  methods: {
    searchCoin() {
      this.filteredCoins = this.coins.filter((coin) => 
        coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) ||
        coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
      )
    },
  },
};
</script>

<style scoped></style>
