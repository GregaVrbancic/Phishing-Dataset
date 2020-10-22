<script>
  import { onMount } from 'svelte'
  import { FacebookLoader, ListLoader } from 'svelte-content-loader'
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
    // console.log(results.data)
    //console.log(parser)
    let row = results.data
    //row['id'] = rowId

    if (rowId == 1) {
      // prepare and store features list
      features = Object.keys(row) //csv.meta['fields']
      columns = features.map((feature) => ({
        display: feature,
        dataName: feature,
        width: 100,
      }))

      console.log(features)
      console.log(columns)
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

    //console.log(csv);
    //console.log(rows);

    // prepare and store features list
    /*features = csv.meta['fields']
        columns = features.map((feature) => ({
          display: feature,
          dataName: feature,
          width: 100,
        }))*/
    //console.log(columns);

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
          {#if isLoading}
            <ListLoader uniqueKey="propertiesLoader" />
          {:else}
            <div class="configuration-item">
              <p class="title">Configure own dataset variation</p>
              <p class="subtitle">Select the dataset ratio and features.</p>
              <label for="selectPhishing">
                Select nr. of phishing instances:
              </label>
              <input
                id="selectPhishing"
                type="range"
                bind:value={selectedPhishing}
                min="0"
                max="10" />
              {selectedPhishing}/{countPhishing}
              <label for="selectLegitimate">
                Select nr. of phishing instances:
              </label>
              <input
                id="selectLegitimate"
                type="range"
                bind:value={selectedLegitimate}
                min="0"
                max="10" />
              {selectedLegitimate}/{countLegitimate}
              <label for="selectFeatures">Select features:</label>
              <Select
                id="selectFeatures"
                items={features}
                isMulti={true}
                on:select={handleSelect}
                on:clear={handleClear} />
            </div>
          {/if}
        </div>
        <div class="column dataview">
          {#if isLoading}
            <FacebookLoader uniqueKey="tableLoader" />
          {:else}
            <div class="data-item">
              <p class="title">Dataset distribution</p>
              <DistributionChart />
            </div>
            <div class="data-item fill-height">
              <p class="title">Dataset preview</p>
              <DataGrid
                {rows}
                allowColumnReordering={false}
                {columns}
                height="100%" />
            </div>
          {/if}
        </div>
      </div>
    </div>
    <div class="column">
      {#if isLoading}
        <ListLoader uniqueKey="downloadLoader" />
      {:else}
        <div class="download-item">
          <p class="title">Download custom dataset</p>
          <p class="subtitle">Download your custom dataset variation.</p>
        </div>
        <div class="download-item">
          <p class="title">Download dataset</p>
          <p class="subtitle">
            Download full dataset variations from Mendeley Data.
          </p>
          <div>
            <strong>Version 1</strong>
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
      {/if}
    </div>
  </div>
</section>
