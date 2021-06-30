<script>
  import { GithubIcon } from 'svelte-feather-icons'

  let isHamburgerActive = false;


</script>

<style type="text/scss">
$border: #efefef;
  .navbar {
    border-bottom: 1px solid $border;

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
            margin: auto;
        }
      }

      .navbar-burger {
        height: inherit;
      }
    }

    .navbar-menu {
      flex-grow: 0;
    }
  }

  @media screen and (max-width: 1023px) {
    .navbar {
      .navbar-brand {
        .navbar-item {
          flex-grow: 1;
        }
      }

      .navbar-end {
        .navbar-item {
          .buttons {
            a {
              margin: auto;
            }
          }
        }
      }
    }
  }
</style>

<nav class="navbar" role="navigation" aria-label="main navigation">
  <div class="navbar-brand">
    <a class="navbar-item" href="/">
      <h1>Phishing Datasets Web App</h1>
    </a>

    <a role="button" href={"#"} aria-label="menu" aria-expanded="false" on:click="{() => isHamburgerActive = !isHamburgerActive}" class="{isHamburgerActive ? 'navbar-burger burger is-active' : 'navbar-burger burger'}" data-target="navbar">
      <span aria-hidden="true"></span>
      <span aria-hidden="true"></span>
      <span aria-hidden="true"></span>
    </a>
  </div>

  <div id="navbar" class="{isHamburgerActive ? 'navbar-menu is-active' : 'navbar-menu'}">
    <div class="navbar-end">
      <div class="navbar-item">
        <div class="buttons">
          <a href="https://github.com/GregaVrbancic/Phishing-Dataset">
            <GithubIcon size="24" />
          </a>
        </div>
      </div>
    </div>
  </div>
</nav>
