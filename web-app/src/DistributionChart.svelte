<script>
  import FusionCharts from 'fusioncharts'
  import Charts from 'fusioncharts/fusioncharts.charts'
  import FusionTheme from 'fusioncharts/themes/fusioncharts.theme.fusion'
  import SvelteFC, { fcRoot } from 'svelte-fusioncharts'

  fcRoot(FusionCharts, Charts, FusionTheme)

  let dataSource = {
      chart: {
        subCaption: 'Distribution between phishing and legitimate websites instances',
        showValues: '0',
        showPercentInTooltip: '0',
        numberPrefix: '',
        enableMultiSlicing: '1',
        theme: 'fusion',
      },
      data: [
        {
          label: 'Phishing',
          value: '30647',
        },
        {
          label: 'Legitimate',
          value: '58001',
        },,
      ],
    },
    chartConfig = {
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
