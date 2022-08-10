<template>
  <div className="container">
    <div className="row">
      <h1 className="display-1 mt-4">Crypto Market</h1>
      <h4 className="fw-light">{{ msg }}</h4>
      <input 
        type="text" 
        placeholder="Search coin..." 
        className='text-light form-control bg-dark rounded-0 border-0 my-4' 
        @keyup="searchCoin"
        v-model="textSearch"
      />
      <table className="table table-dark table-hover">
        <caption className="caption-top">Powered by <a href="https://www.coingecko.com/" className="link-light text-decoration-none">CoinGecko</a></caption>
        <thead>
          <tr>
            <th v-for="(title,index) in titles" :key="index">{{title}}</th>
          </tr>
        </thead>
        <tbody className="table-group-divider">
          <tr v-for="(coin, index) in filteredCoins" :key="coin.id">
            <td className="text-muted">
              {{index+1}}
            </td>
            <td>
              <img :src="coin.image" style="width: 2rem" className="me-2" alt="a coin thumbnail">
              <span>
                {{coin.name}}
              </span>
              <span className="ms-2 text-uppercase text-muted">
                {{coin.symbol}}
              </span>
            </td>
            <td>
              $ {{coin.current_price}}
            </td>
            <td :className="[coin.price_change_percentage_24h>0 ? 'text-success' : 'text-danger']">
              {{coin.price_change_percentage_24h.toFixed(2)}} %
            </td>
            <td>
              $ {{coin.total_volume.toLocaleString()}}
            </td>
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
      msg: "Hello Crypto users!",
      coins: [],
      filteredCoins:[],
      titles: ['#','Coin','Price','Price Change 24h','24h volume'],
      textSearch: ""
    };
  },
  async mounted() {
    const res = await fetch(
      "https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false"
    );
    const data = await res.json();
    this.coins = data;
    this.filteredCoins = data;
  },
  methods:{
    searchCoin(){
      this.filteredCoins = this.coins.filter(coin => 
        coin.name.toLowerCase().includes(this.textSearch.toLowerCase()) ||
        coin.symbol.toLowerCase().includes(this.textSearch.toLowerCase())
      );
    },
  },
};
</script>

<style></style>