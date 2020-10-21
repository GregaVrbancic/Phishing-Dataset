<script>
  import { onMount } from 'svelte'
  import { FacebookLoader, ListLoader } from 'svelte-content-loader'
  import Select from 'svelte-select'
  import Papa from 'papaparse'
  import DataGrid from 'svelte-data-grid'

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
          step: step,
        })
        selectedPhishing = countPhishing
        selectedLegitimate = countLegitimate

        //console.log(csv);
        //console.log(rows);

        // prepare and store features list
        features = csv.meta['fields']
        columns = features.map((feature) => ({
          display: feature,
          dataName: feature,
          width: 100,
        }))
        //console.log(columns);

        isLoading = false
      })
    // console.log(promise.body);
  })

  const step = (results, parser) => {
    //console.log(results.data)
    let row = results.data
    row['id'] = rowId

    if (row['phishing'] == 1) {
      countPhishing++
    } else {
      countLegitimate++
    }

    rows.push(row)

    rowId++
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
    height: calc(100vh - (19.25rem - 0.75rem));
  }

  .dataview {
    display: flex;
    flex: 1 1 50%;
    flex-direction: column;
    border-left: 1px ridge $border;

    .data-item {
    }

    .fill-height {
        flex: 1 1 100%;
    }
  }

  .download-item, .configuration-item {
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
              <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. In purus ante, pretium non pellentesque quis, suscipit sit amet dui. Phasellus fermentum, nisi quis pellentesque tincidunt, sapien est accumsan nunc, vel convallis leo lorem sed metus. In nec accumsan neque, at euismod risus. Vivamus pellentesque nulla at leo facilisis maximus. Quisque malesuada nisi et convallis rhoncus. Nullam sapien mi, dapibus ut enim in, pharetra ornare augue. Vestibulum bibendum pretium mi, sit amet dictum lacus convallis ut. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Vivamus quam nunc, pulvinar sed mauris a, facilisis bibendum massa. Vivamus sit amet pulvinar neque. Aenean libero nisi, ornare nec libero et, iaculis lobortis tortor.

Nunc sagittis est purus, vel pellentesque nisl egestas sit amet. Nunc et nisi ac enim eleifend laoreet. Pellentesque porta velit turpis, et euismod diam gravida vitae. Nulla molestie mauris at arcu consectetur malesuada. Nunc vehicula consectetur augue. Nulla fermentum auctor neque et semper. Morbi porttitor metus justo, id commodo est aliquam ut.

Nulla ut varius ipsum, id ornare eros. Aliquam quis libero blandit, eleifend nibh quis, tincidunt lectus. Praesent iaculis sodales finibus. Suspendisse feugiat dictum magna. Nam sed dictum nulla. Etiam nec magna tincidunt, sollicitudin risus non, pulvinar nibh. Aenean consequat eu nulla ac laoreet. Ut vehicula, est id rhoncus congue, dui erat pharetra arcu, congue commodo purus odio sed elit. Duis vel ex in sapien scelerisque lacinia. Vestibulum eget mauris mollis lacus interdum pretium.

Donec laoreet consequat ipsum sed volutpat. Duis nec elit rhoncus, condimentum ipsum sit amet, pretium quam. Maecenas vel aliquet turpis. Suspendisse at cursus ipsum. Pellentesque ullamcorper diam odio, at semper velit consequat non. Praesent dignissim congue neque, porta tincidunt ipsum mollis sit amet. Quisque consequat, mi ac mattis imperdiet, velit eros efficitur ligula, vel finibus nunc arcu ut tortor. Cras eget massa laoreet mi fringilla tempus ac vitae ante. Nunc eget erat vitae ex aliquet sagittis id a dui. Vivamus consectetur dui nulla, id mattis ante lobortis vitae. Vivamus scelerisque tellus lacus, ut pulvinar nibh tristique eget. Fusce rhoncus risus sed tincidunt porta. Ut sit amet metus venenatis, semper nisl quis, efficitur mauris. Suspendisse ac ipsum eget ligula varius venenatis. Nam porttitor efficitur ante. Donec iaculis turpis ac metus congue, ac pharetra nisl vestibulum.

In bibendum erat augue, vitae fringilla nulla scelerisque eget. Aliquam placerat quam ante, eu suscipit sem varius sit amet. Vestibulum aliquet sit amet purus eget tempor. Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia curae; Curabitur mattis semper ex, sit amet consectetur nunc maximus at. Donec commodo egestas convallis. Nunc porttitor est ac vestibulum consectetur. Sed sagittis purus at nulla ullamcorper dapibus. In eget diam dictum, auctor turpis a, varius erat. Vivamus risus lacus, consectetur eu dignissim nec, euismod quis lacus. Nam feugiat enim massa, id lacinia purus euismod semper. Sed dignissim mi nulla, at cursus ex commodo sit amet. Aliquam urna nisl, condimentum eget commodo ac, blandit eleifend risus. In nec elit et nunc pulvinar semper. Nulla varius eros urna. Aliquam mollis tellus dignissim leo tristique, ut sollicitudin turpis pellentesque.</p>
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
