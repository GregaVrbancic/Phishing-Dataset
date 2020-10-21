<script>
  import { GithubIcon } from 'svelte-feather-icons'
</script>

<style type="text/scss">
  .navbar {
    border-bottom: 1px solid #efefef;

    .navbar-brand {
      display: flex;
      flex-grow: 1;
      justify-content: center;

      .navbar-item {
        h1 {
            color: #ff3e00;
            text-transform: uppercase;
            font-size: x-large;
            font-weight: 300;
            line-height: 3rem;
        }
      }
    }

    .navbar-menu {
      flex-grow: 0;
    }
  }




</style>

<nav class="navbar" role="navigation" aria-label="main navigation">
  <div class="navbar-brand">
    <a class="navbar-item" href="/">
      <h1>Phishing Datasets Web App</h1>
    </a>
  </div>

  <div id="navbar" class="navbar-menu">
    <div class="navbar-end">
      <div class="navbar-item">
        <div class="buttons">
          <a>
            <GithubIcon size="24" />
          </a>
        </div>
      </div>
    </div>
  </div>
</nav>
