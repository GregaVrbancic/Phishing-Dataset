<script>
  import FusionCharts from 'fusioncharts'
  import Charts from 'fusioncharts/fusioncharts.charts'
  import FusionTheme from 'fusioncharts/themes/fusioncharts.theme.fusion'
  import SvelteFC, { fcRoot } from 'svelte-fusioncharts'

  fcRoot(FusionCharts, Charts, FusionTheme)

  export let data

  $: dataSource = {
    chart: {
      subCaption:
        'Distribution between phishing and legitimate websites instances',
      showValues: '0',
      showPercentInTooltip: '0',
      numberPrefix: '',
      enableMultiSlicing: '1',
      theme: 'fusion',
    },
    data: data,
  }

  $: chartConfig = {
    type: 'pie3d',
    renderAt: 'chart-container',
    dataSource,
  }
</script>

<style type="text/scss">
  div#chart-container {
    text-align: center;
  }
</style>

<div id="chart-container">
  <SvelteFC {...chartConfig} />
</div>
