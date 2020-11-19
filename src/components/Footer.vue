<template>
  <footer class="footer">
    <div class="copyright">
      <span class="copyright__text">Copyright © 2016-2020. AnimaRender</span>
    </div>
    <nav class="links">
      <ul>
        <li class="link"><a href="#">Help</a></li>
        <li class="link"><a href="#">Software</a></li>
        <li class="link"><a href="#">Referral program</a></li>
      </ul>
    </nav>
  </footer>
</template>

<script>
  export default {
    name: "Footer"
  }
</script>

<style lang="scss" scoped>
  .footer {
    min-height: 55px;
    background: rgba(72, 100, 230, 0.1);

    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .copyright__text {
    font-size: 12px;
    line-height: 15px;
    color: #86868B;
    margin-left: 95px;
  }

  .links {
    margin-right: 50px;

    ul {
      display: flex;
    }

    a {
      font-size: 12px;
      line-height: 15px;
      color: #86868B;
      text-decoration: none;
      margin-left: 30px;
    }
  }

  @media (max-width: 768px) {
    .footer {
      flex-direction: column;
    }

    .copyright {
      order: 1;
      background-color: #E0E0E0;
      width: 100%;
      height: 47px;
      display: flex;
      justify-content: center;
      align-items: center;

      &__text {
        margin: 0;
      }
    }

    .links {
      margin: 0;

      ul {
        flex-direction: column;
        justify-content: center;
        align-items: center;
      }

      a {
        margin-left: 0;
      }

      .link {
        margin-top: 20px;
      }

      .link:last-child {
        margin-bottom: 20px;
      }
    }
  }


</style>