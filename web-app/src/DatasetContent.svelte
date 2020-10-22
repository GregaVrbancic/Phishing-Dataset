<script>
  import { onMount } from 'svelte'
  import { InstagramLoader, ListLoader } from 'svelte-content-loader'
  import Select from 'svelte-select'
  import Papa from 'papaparse'
  import DataGrid from 'svelte-data-grid'

  import DistributionChart from './DistributionChart.svelte'

  let isLoading = true

  let features = []
  let rows = []
  let columns = []
  let rowId = 1
  let countPhishing = 0
  let countLegitimate = 0
  let selectedFeatures = []
  let selectedPhishing = 0
  let selectedLegitimate = 0
  $: distributionChartData = [
    {
      label: 'Phishing',
      value: selectedPhishing,
    },
    {
      label: 'Legitimate',
      value: selectedLegitimate,
    },
  ]

  onMount(async () => {
    console.log('onMount')
    fetch(
      `https://raw.githubusercontent.com/GregaVrbancic/Phishing-Dataset/master/dataset_full.csv`,
    )
      .then((response) => {
        return response.text()
      })
      .then((data) => {
        let csv = Papa.parse(data, {
          header: true,
          worker: true,
          step: onStep,
          complete: onComplete,
        })
      })
  })

  const onStep = (results, parser) => {
    let row = results.data

    if (rowId == 1) {
      // prepare and store features list
      features = Object.keys(row)
      columns = features.map((feature) => ({
        display: feature,
        dataName: feature,
        width: 100,
      }))
    }

    if (row['phishing'] == 1) {
      countPhishing++
    } else {
      countLegitimate++
    }

    rows.push(row)

    rowId++
  }

  const onComplete = (results, file) => {
    console.log('Parsing complete')
    selectedPhishing = countPhishing
    selectedLegitimate = countLegitimate

    isLoading = false
  }

  const handleSelect = (selectedVal) => {
    selectedFeatures = selectedVal.detail
    console.log(selectedFeatures)
  }

  const handleClear = () => {
    selectedFeatures = []
    console.log(selectedFeatures)
  }
</script>

<style type="text/scss">
  $border: #efefef;
  /* Override bulma */
  .title {
    font-size: 1rem;
  }

  .subtitle {
    font-size: 0.85rem;
    font-style: italic;
  }

  /* Custom */
  .main {
    border-right: 1px ridge $border;
  }

  .is-fullheight.columns {
    height: calc(100vh - (20rem - 0.75rem));
  }

  .dataview {
    display: flex;
    flex: 1 1 50%;
    flex-direction: column;
    border-left: 1px ridge $border;

    .data-item {
      margin-bottom: 3rem;
      text-align: center;
    }

    .fill-height {
      flex: 1 1 100%;
    }
  }

  .download-item,
  .configuration-item {
    padding-bottom: 1.5rem;
    margin-bottom: 1.5rem;
    font-size: 0.8rem;
  }

  .download-item:nth-child(odd) {
    border-bottom: 1px ridge $border;
  }
</style>

<section class="section">
  <div class="columns is-desktop is-fullheight">
    <div class="column is-four-fifths main">
      <div class="columns full-height">
        <div class="column is-one-quarter">
          <div class="configuration-item">
            <p class="title">Configure own dataset variation</p>
            {#if isLoading}
              <ListLoader uniqueKey="propertiesLoader" />
            {:else}
              <p class="subtitle">Select the dataset ratio and features.</p>
              <label for="selectPhishing">
                Select nr. of phishing instances:
              </label>
              <input
                id="selectPhishing"
                type="range"
                bind:value={selectedPhishing}
                min="0"
                max={countPhishing} />
              {selectedPhishing}/{countPhishing}
              <label for="selectLegitimate">
                Select nr. of phishing instances:
              </label>
              <input
                id="selectLegitimate"
                type="range"
                bind:value={selectedLegitimate}
                min="0"
                max={countLegitimate} />
              {selectedLegitimate}/{countLegitimate}
              <label for="selectFeatures">Select features:</label>
              <Select
                id="selectFeatures"
                items={features}
                isMulti={true}
                on:select={handleSelect}
                on:clear={handleClear} />
            {/if}
          </div>
        </div>
        <div class="column dataview">
          <div class="data-item">
            <p class="title">Dataset distribution</p>
            {#if isLoading}
              <InstagramLoader uniqueKey="distributionLoader" />
            {:else}
              <DistributionChart bind:data={distributionChartData} />
            {/if}
          </div>
          <div class="data-item fill-height">
            <p class="title">Dataset preview</p>
            {#if isLoading}
              <ListLoader uniqueKey="tableLoader" />
            {:else}
              <DataGrid
                {rows}
                allowColumnReordering={false}
                {columns}
                height="100%" />
            {/if}
          </div>
        </div>
      </div>
    </div>
    <div class="column">
      <div class="download-item">
        <p class="title">Download custom dataset</p>
        {#if isLoading}
          <ListLoader uniqueKey="downloadLoader" />
        {:else}
          <p class="subtitle">Download your custom dataset variation.</p>
          <div style="text-align:center;">
            <a class="button is-success">Download</a>
          </div>
        {/if}
      </div>
      <div class="download-item">
        <p class="title">Download dataset</p>
        <p class="subtitle">
          Download full dataset variations from Mendeley Data.
        </p>
        <div>
          <strong>Version 1</strong>
          <ul>
            <li>
              <a
                href="https://data.mendeley.com/public-files/datasets/72ptz43s9v/files/26197eb8-15bc-4e06-a269-aa10ddc286f0/file_downloaded">
                <strong>dataset_full</strong>
              </a>
            </li>
            <li>
              <a
                href="https://data.mendeley.com/public-files/datasets/72ptz43s9v/files/154607af-0e5d-487d-87e4-c5070b2eb544/file_downloaded">
                <strong>dataset_small</strong>
              </a>
            </li>
          </ul>
          <p>
            Published:
            <strong>24-09-2020</strong>
          </p>
          <p>
            DOI:
            <strong>
              <a href="http://dx.doi.org/10.17632/72ptz43s9v.1">
                10.17632/72ptz43s9v.1
              </a>
            </strong>
          </p>
        </div>
      </div>
    </div>
  </div>
</section>
