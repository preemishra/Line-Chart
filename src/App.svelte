<script>
  import * as d3 from 'd3';
  import Header from './components/Header.svelte';
  import LineChart from './components/LineChart.svelte';
  import Footer from './components/Footer.svelte';

  let lineChartData;

  d3.csv('../data/temperatures.csv').then(data => (lineChartData = data));
</script>

<main>
  <div class="app">
    <Header />
    {#if lineChartData}
      <LineChart data={lineChartData} />
    {/if}
    <Footer />
  </div>
</main>

<style>
  @font-face {
    font-family: 'Merriweather';
    font-style: normal;
    font-weight: 700;
    src: local(''),
      url('../fonts/merriweather-v25-latin-700.woff2') format('woff2'),
      url('../fonts/merriweather-v25-latin-700.woff') format('woff');
  }

  @font-face {
    font-family: 'Lato';
    font-style: normal;
    font-weight: 400;
    src: local(''), url('../fonts/lato-v20-latin-regular.woff2') format('woff2'),
      url('../fonts/lato-v20-latin-regular.woff') format('woff');
  }

  :root {
    font-family: 'Lato', sans-serif;
    font-size: 16px;
    color: #282828;
    background: #ffffff;
    margin: 0 15px;
  }

  .app {
    display: flex;
    flex-direction: column;
    align-items: center;
    height: 100%;
    width: 100%;
    user-select: none;
  }
</style>
