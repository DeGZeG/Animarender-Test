<template>
  <div class="hamburger-menu">
    <input id="menu__toggle" class="menu__toggle" type="checkbox" />
    <label class="menu__btn" for="menu__toggle">
      <span></span>
    </label>

    <div class="menu__box">
      <div class="menu__top">
        <div class="menu__language">
          <div class="language">
            <img src="../assets/images/flag.svg" alt="language">
          </div>
        </div>
        <div class="menu__user">
          <img class="user__avatar" src="../assets/images/avatar.png" alt="avatar">
          <span class="user__name">a.nikitina@animarender.com</span>
        </div>
        <div class="menu__logout">
          <img src="../assets/images/logout.svg" alt="logout">
        </div>
      </div>

      <nav class="menu__nav">
        <ul>
          <li class="nav__item nav__item_active"><img class="img-svg" src="../assets/images/link1.svg" alt="link"></li>
          <li class="nav__item"><img class="img-svg" src="../assets/images/link2.svg" alt="link"></li>
          <li class="nav__item"><img class="img-svg" src="../assets/images/link3.svg" alt="link"></li>
          <li class="nav__item"><img class="img-svg" src="../assets/images/link4.svg" alt="link"></li>
          <li class="nav__item"><img class="img-svg" src="../assets/images/link5.svg" alt="link"></li>
          <li class="nav__item"><img class="img-svg" src="../assets/images/link6.svg" alt="link"></li>
          <li class="nav__item"><img class="img-svg" src="../assets/images/link7.svg" alt="link"></li>
        </ul>
      </nav>
    </div>
  </div>
</template>

<script>
  export default {
    name: "Menu"
  }
</script>

<style lang="scss" scoped>
  .menu__toggle {
    position: absolute;
    z-index: -1;
    opacity: 0;
  }

  .menu__toggle:checked ~ .menu__btn > span {
    transform: rotate(45deg);
  }
  .menu__toggle:checked ~ .menu__btn > span::before {
    top: 0;
    transform: rotate(0);
  }
  .menu__toggle:checked ~ .menu__btn > span::after {
    top: 0;
    transform: rotate(90deg);
  }
  .menu__toggle:checked ~ .menu__box {
    display: block;
    animation: move-box .25s 1 forwards alternate;
  }

  .menu__btn {
    display: flex;
    position: relative;
    align-items: center;

    width: 18px;
    height: 12px;

    cursor: pointer;
    z-index: 1;
  }

  .menu__btn > span,
  .menu__btn > span::before,
  .menu__btn > span::after {
    display: block;
    position: absolute;

    width: 100%;
    height: 2px;

    background-color: #526AE5;

    transition-duration: .25s;
  }
  .menu__btn > span {

    &::before {
      content: '';
      top: -5px;
    }

    &::after {
      content: '';
      top: 5px;
    }
  }

  .menu__box {
    display: none;
    position: absolute;

    top: 84px;
    right: -100%;
    z-index: 10;

    width: 250px;

    background-color: #ECEFF1;
    box-shadow: 1px 0 6px rgba(0, 0, 0, .2);
  }

  .nav__item {
    position: relative;
    width: 100%;
    height: 60px;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .nav__item_active {
    background-color: #F5F6FC;
  }

  .menu {

    &__top {
      display: flex;
      justify-content: space-around;
      align-items: center;
      margin: 15px 0;
    }

    &__language {
      position: relative;

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

    &__user {
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;

      .user__avatar {
        width: 36px;
        height: 36px;
        border-radius: 50%;
        object-fit: cover;
        margin-bottom: 5px;
      }

      .user__name {
        font-size: 0.6rem;
        line-height: 20px;
        color: #526AE5;
        margin-left: 18px;
      }
    }

    &__logout {

    }
  }
  
  @keyframes move-box {
    0% {
      right: -100%;
    }

    100% {
      right: 0;
    }
  }
</style>