<template>
  <div class="SiteHeader">
    <div>
      <router-link :to="{ name: 'home' }" class="Logo">vue-scuffka</router-link>
    </div>
    <div>
      <nav class="SiteHeader-nav">
        <router-link :to="{ name: 'editor' }" class="SiteHeader-link">
          <span class="Button is-light">Try it</span>
        </router-link>

        <a
          href="https://github.com/anthonykoch/vue-scuffka"
          class="SiteHeader-link"
          style="display: block"
        >
          <svg class="SiteHeader-icon is-github" aria-labelledby="simpleicons-github-icon" role="img" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><title id="simpleicons-github-icon">View on Github</title><path d="M12 .297c-6.63 0-12 5.373-12 12 0 5.303 3.438 9.8 8.205 11.385.6.113.82-.258.82-.577 0-.285-.01-1.04-.015-2.04-3.338.724-4.042-1.61-4.042-1.61C4.422 18.07 3.633 17.7 3.633 17.7c-1.087-.744.084-.729.084-.729 1.205.084 1.838 1.236 1.838 1.236 1.07 1.835 2.809 1.305 3.495.998.108-.776.417-1.305.76-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.465-2.38 1.235-3.22-.135-.303-.54-1.523.105-3.176 0 0 1.005-.322 3.3 1.23.96-.267 1.98-.399 3-.405 1.02.006 2.04.138 3 .405 2.28-1.552 3.285-1.23 3.285-1.23.645 1.653.24 2.873.12 3.176.765.84 1.23 1.91 1.23 3.22 0 4.61-2.805 5.625-5.475 5.92.42.36.81 1.096.81 2.22 0 1.606-.015 2.896-.015 3.286 0 .315.21.69.825.57C20.565 22.092 24 17.592 24 12.297c0-6.627-5.373-12-12-12"/></svg>
        </a>
      </nav>
    </div>
  </div>
</template>

<style scoped lang="scss">

.SiteHeader {
  align-items: center;
  background-color: #f29100;
  background-color: #827b70;
  // background-color: #2b292b;
  color: white;
  display: flex;
  justify-content: space-between;
  font-family: Oswald;
  font-size: 13px;
  padding-bottom: 1rem;
  padding-left: 4rem;
  padding-right: 4rem;
  padding-top: 1rem;
  text-transform: uppercase;

  > span {
    background-color: inherit;
    padding: 2px 0px;
    position: relative;
    z-index: 1;

    // &:before {
    //   content: '';
    //   position: absolute;
    //   border-radius: 50%;
    //   border: 2px solid white;
    //   left: 0;
    //   top: 50%;
    //   transform: translate(-50%, -50%);
    //   height: 30px;
    //   width: 30px;
    //   z-index: -1;
    // }
  }
}

.SiteHeader-nav {
  align-items: center;
  display: flex;
}

.SiteHeader-link {
  margin-left: 30px;
}

.Logo {
  color: white;
  text-decoration: underline;
  letter-spacing: 2px;

  svg {
    vertical-align: middle;
  }

  &:link,
  &:visited,
  &:hover,
  &:active {
    color: white;
  }
}

.SiteHeader-icon {
  fill: white;
  vertical-align: middle;
  width: 30px
}

@media only screen and (max-width: 720px) {
  .SiteHeader {
    padding-left: 1rem;
    padding-right: 1rem;
  }
}

</style>
