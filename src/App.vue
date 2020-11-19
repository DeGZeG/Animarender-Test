<template>
  <div id="root" class="root">
    <div class="container">
      <Header />
      <div class="app">
        <Aside />
        <Main />
      </div>
      <Footer />
    </div>
  </div>
</template>

<script>
  import Header from "./components/Header";
  import Aside from "./components/Aside";
  import Main from "./components/Main";
  import Footer from "./components/Footer";
  export default {
    name: 'App',
    components: {Footer, Main, Aside, Header},
  }
</script>

<style lang="scss">
  @import '~@/assets/scss/fonts.scss';

  *, *::before, *::after {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'GoogleSans', sans-serif;
  }

  li {
    list-style: none;
  }

  html {
    background: #FDFDFD;
  }

  .container {
    max-width: 1200px;
    height: 100%;
    margin: 0 auto;
  }

  .app {
    display: flex;
    height: calc(100vh - 71px - 55px);
  }

  .nav__item_active path {
      fill: #526AE5;
  }

  @media (max-width: 768px) {
    html {
      background-color: #F6F7FC;
    }

    .app {
      height: auto;
    }
  }
</style>
