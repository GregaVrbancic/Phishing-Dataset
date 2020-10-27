<script>
  import Modal from 'svelte-simple-modal';

  import NavBar from './NavBar.svelte'
  import DatasetContent from './DatasetContent.svelte'
</script>

<style type="text/scss" global>
  @import 'bulma/bulma';

  $primary: #ff3e00;
  $secondary: #af2d02;

  :global(a),
  :global(a:visited) {
    color: $primary;
    &:hover {
      text-decoration: none;
      color: $secondary;
    }
  }

  :global(.full-height) {
    height: 100%;
  }
</style>

<main>

  <NavBar />

  <Modal>
    <DatasetContent />
  </Modal>

  <footer class="footer">
    <div class="content has-text-centered">
      <p>
        <strong>Phishing Dataset Web App</strong>
        by
        <a href="https://grega.xyz">Grega Vrbančič</a>
        . The source code is licensed
        <a href="http://opensource.org/licenses/mit-license.php">MIT</a>
        .
      </p>
    </div>
  </footer>
</main>
