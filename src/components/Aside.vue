<template>
  <div class="wrapper">
    <aside class="menu">
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
    </aside>
  </div>
</template>

<script>
  export default {
    name: "Aside",
    mounted() {
      $('img.img-svg').each(function(){
        let $img = $(this);
        let imgClass = $img.attr('class');
        let imgURL = $img.attr('src');
        $.get(imgURL, function(data) {
          let $svg = $(data).find('svg');
          if(typeof imgClass !== 'undefined') {
            $svg = $svg.attr('class', imgClass+' replaced-svg');
          }
          $svg = $svg.removeAttr('xmlns:a');
          if(!$svg.attr('viewBox') && $svg.attr('height') && $svg.attr('width')) {
            $svg.attr('viewBox', '0 0 ' + $svg.attr('height') + ' ' + $svg.attr('width'))
          }
          $img.replaceWith($svg);
        }, 'xml');
      });
    }
  }
</script>

<style lang="scss" scoped>
  .menu {
    width: 100px;
    min-height: 550px;
    background-color: white;

    &__nav {
      padding: 20px 0;
    }
  }

  .nav__item {
    position: relative;
    width: 100px;
    height: 60px;
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .nav__item:hover::after {
    content: '';
    width: 3px;
    height: 100%;
    background-color: #526AE5;
    border-radius: 4px;
    position: absolute;
    right: 0;
  }

  .nav__item_active {
    background-color: #F5F6FC;
  }

  @media (max-width: 768px) {
    .menu {
      display: none;
    }
  }
</style>