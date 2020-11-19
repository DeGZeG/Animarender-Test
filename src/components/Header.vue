<template>
  <header class="header">
    <div class="header__wrapper">
      <div class="header__logo"><a href="#"><img src="../assets/images/logo.svg" alt="logo"></a></div>
      <div class="header__user">
        <img class="user__avatar" src="../assets/images/avatar.png" alt="avatar">
        <span class="user__name">a.nikitina@animarender.com</span>
      </div>
      <ul class="header__menu">
        <li class="menu__item menu__ap">
          <span class="ap">741.19 AP</span>
        </li>
        <li class="menu__item">
          <div class="menu__notifications menu__notifications_exist">
            <img src="../assets/images/alarm.svg" alt="notifications">
          </div>
        </li>
        <li class="menu__item menu__language">
          <div class="language">
            <img src="../assets/images/flag.svg" alt="language">
          </div>
        </li>
        <li class="menu__item menu__logout">
          <img src="../assets/images/logout.svg" alt="logout">
        </li>
        <li class="menu__item menu__menu">
<!--          <img src="../assets/images/menu.svg" alt="menu">-->
          <Menu />
        </li>
      </ul>
    </div>
  </header>
</template>

<script>
  import Menu from "./Menu";
  export default {
    name: "Header",
    components: {Menu}
  }
</script>

<style lang="scss" scoped>
  .header {
    width: 100%;
    height: 71px;
    background-color: white;

    &__wrapper {
      height: 100%;
      display: flex;
      align-items: center;
      justify-content: space-between;
    }

    &__logo {
      margin-left: 29px;
    }

    &__user {
      display: flex;
      align-items: center;
      padding: 0 20px;
    }
  }

  .user__avatar {
    width: 36px;
    height: 36px;
    border-radius: 50%;
    object-fit: cover;
  }

  .user__name {
    line-height: 20px;
    color: #526AE5;
    margin-left: 18px;
  }

  .header__menu {
    display: flex;
    height: 100%;
  }

  .menu {
    &__item {
      width: 100px;
      display: flex;
      justify-content: center;
      align-items: center;

      img {
        cursor: pointer;
      }
    }

    &__item:not(:first-child) {
      border-left: 1px solid #E5E5E5;
    }

    &__ap {

      .ap {
        line-height: 20px;
        color: #A7ABC3;
      }
    }

    &__notifications {
      position: relative;
    }

    &__notifications_exist::before {
      content: '';
      width: 5px;
      height: 5px;
      background: #FD27EB;
      border-radius: 50%;
      display: block;
      position: absolute;
      left: 50%;
      top: -10px;
      transform: translate(-50%, 0);
    }

    &__language {
      .language {
        position: relative;
      }

      .language::after {
        content: '';
        width: 8px;
        height: 5px;
        background-image: url("../assets/images/arrow.svg");
        position: absolute;
        top: 50%;
        right: -13px;
        transform: translate(0, -50%);
      }
    }

    &__menu {
      display: none;
    }
  }

  @media (max-width: 768px) {
    .header {
      height: 84px;
      border-top: 24px solid #C4C4C4;
      box-shadow: 0 4px 20px rgba(0, 0, 0, 0.25);

      &__logo {
        margin-left: 17px;

        img {
          width: 16px;
          height: 16px;
        }
      }

      &__user {
        display: none;
      }
    }

    .menu {

      &__language {
        display: none;
      }

      &__logout {
        display: none;
      }

      &__menu {
        display: flex;
        margin-right: 15px;
      }

      &__notifications {
        margin: 0 20px;
      }

      &__item {
        width: auto
      }

      &__item:not(:first-child) {
        border-left: none;
      }

      &__ap {

        .ap {
          font-size: 15px;
          color: #526AE5;
        }
      }
    }
  }
</style>