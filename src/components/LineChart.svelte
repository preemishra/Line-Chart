<script>
  import * as d3 from 'd3';
  import Chart from './Chart/Chart.svelte';
  import Line from './Chart/Line.svelte';
  import XAxis from './Chart/XAxis.svelte';
  import YAxis from './Chart/YAxis.svelte';

  export let data;

  const xAccessor = d => parseDate(d.date);
  const yAccessor = d => +d.temperature;

  const parseDate = d3.timeParse('%Y-%m-%d');
  const formatDate = d3.timeFormat('%-b %-d');

  let width = 100;
  let height = 100;

  const margins = {
    marginTop: 40,
    marginRight: 30,
    marginBottom: 65,
    marginLeft: 45,
  };

  $: dms = {
    width,
    height,
    ...margins,
    boundedHeight: Math.max(
      height - margins.marginTop - margins.marginBottom,
      0,
    ),
    boundedWidth: Math.max(width - margins.marginLeft - margins.marginRight, 0),
  };

  $: xScale = d3
    .scaleTime()
    .domain(d3.extent(data, xAccessor))
    .range([0, dms.boundedWidth]);

  $: yScale = d3
    .scaleLinear()
    .domain([0, d3.max(data, yAccessor)])
    .range([dms.boundedHeight, 0])
    .nice();

  $: xAccessorScaled = d => xScale(xAccessor(d));
  $: yAccessorScaled = d => yScale(yAccessor(d));
</script>

<div
  class="line-chart__placeholder"
  bind:clientWidth={width}
  bind:clientHeight={height}
>
  <Chart dimensions={dms}>
    <Line {data} xAccessor={xAccessorScaled} yAccessor={yAccessorScaled} />

    <XAxis scale={xScale} label="day" formatTick={formatDate} />

    <YAxis scale={yScale} label="max temperature (&#xb0;C)" />
  </Chart>
</div>

<style>
  .line-chart__placeholder {
    position: relative;
    font-size: 16px;
    width: 100%;
    height: 300px;
    max-width: 900px;
    margin: 0 40px;
  }
</style>
