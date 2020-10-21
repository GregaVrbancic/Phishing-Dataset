<script>
  import NavBar from './NavBar.svelte';
</script>

<style type="text/scss" global>
@import "bulma/bulma";

  :global(a) {
    color: #ff3e00;
    &:hover {
      text-decoration: none;
    }
  }

  .title {
    font-size: 1rem;
  }

  .subtitle {
    font-size: 1rem;
    font-style: italic;
  }
</style>

<main>

  <NavBar />

  <section class="section">
	<div class="columns is-desktop">
		<div class="column is-three-quarters">
		  <div class="columns">
			<div class="column is-one-third">
				<p class="title">Configure own dataset variation</p>
				<p class="subtitle">Select the dataset ratio and features.</p>
			</div>
			<div class="column"><p class="title">Dataset variation preview</p></div>
		  </div>
		</div>
		<div class="column">
			<p class="title">Download the original dataset variations</p>
		</div>
	  </div>
  </section>

  <footer class="footer">
    <div class="content has-text-centered">
      <p>
        <strong>Phishing Dataset Web App</strong>
        by
        <a href="https://grega.xyz">Grega Vrbančič</a>
        . The source code is licensed
        <a href="http://opensource.org/licenses/mit-license.php">MIT</a>
        . The website content.
      </p>
    </div>
  </footer>
</main>
