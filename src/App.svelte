<script>
  import svelteLogo from './assets/svelte.svg'
  import Counter from './lib/Counter.svelte'
  import { onMount } from "svelte";

  let coins = [];
  let headings = ["#", "Coin", "Price", "Price Change", "24h Volume"];
  let textSearch = "";
  let filteredCoins = [];

  let ref = null;


  const loadCoins = async () => {
    const res = await fetch(
  'https://api.coingecko.com/api/v3/coins/markets?vs_currency=eur&order=market_cap_desc&per_page=100&page=1&sparkline=false'
    );
    const data = await res.json();
    coins = data;
    filteredCoins = data;
  };
  loadCoins();

  const searchCoin = (value) => {
    filteredCoins = coins.filter(
      (coin) =>
        coin.name.toLowerCase().includes(value) ||
        coin.symbol.toLowerCase().includes(value)
    );
  };

  onMount(() => {
    ref.focus();
  });

</script>

<main class="container p-4">

  <div>
    <a href="https://vitejs.dev" target="_blank"> 
      <img src="/vite.svg" class="logo" alt="Vite Logo" />
    </a>
    <a href="https://svelte.dev" target="_blank"> 
      <img src={svelteLogo} class="logo svelte" alt="Svelte Logo" />
    </a>
  </div>
  <h1>Vite + Svelte</h1>

  <div class="card">
    <Counter />
  </div>

  <p>
    Check out <a href="https://github.com/sveltejs/kit#readme" target="_blank">SvelteKit</a>, the official Svelte app framework powered by Vite!
  </p>

  <p class="read-the-docs">
    Click on the Vite and Svelte logos to learn more
  </p>


  <h1>CoinMarket</h1>
  <table class="table table-dark table-hover">
    <thead>
      <tr>
        {#each headings as heading}
          <th>{heading}</th>
        {/each}
      </tr>
    </thead>
    <tbody>
      {#each filteredCoins as coin, i}
        <tr>
          <td class="text-muted">{i}</td>
          <td>
            <img
              src={coin.image}
              alt={coin.name}
              style="width: 2rem"
              class="img-fluid"
            />
            <span>
              {coin.name}
            </span>
            <span class="ms-3 text-muted text-uppercase">
              {coin.symbol}
            </span>
          </td>
          <td>
            ${coin.current_price.toLocaleString()}
          </td>
          <td
            class={coin.price_change_percentage_24h > 0
              ? "text-success"
              : "text-danger"}
          >
            {coin.price_change_percentage_24h}
          </td>
          <td>
            {coin.total_volume.toLocaleString()}
          </td>
        </tr>
      {/each}
    </tbody>
  </table>
</main>






<style>
  .logo {
    height: 6em;
    padding: 1.5em;
    will-change: filter;
  }
  .logo:hover {
    filter: drop-shadow(0 0 2em #646cffaa);
  }
  .logo.svelte:hover {
    filter: drop-shadow(0 0 2em #ff3e00aa);
  }
  .read-the-docs {
    color: #888;
  }
</style>
