<template>
  <main id="main" class="main">
    <section class="spec">
      <div class="render">
        <div class="render__title">
          <span>Your renderer</span>
        </div>
        <div class="render__image">
          <img src="../assets/images/render_icon.svg" width="51" height="58" alt="render">
        </div>
      </div>

      <div class="order-pic">
        <img src="../assets/images/123.svg" alt="order">
      </div>

      <div class="details">
        <div class="details__title">
          <span>Specify your project details</span>
        </div>
        <div class="radios">
          <div class="radio">
            <input name="spec" id="animation_radio" type="radio" value="animation"
              v-model="sceneType"
            />
            <label for="animation_radio" class="radio__name">Animation</label>
          </div>
          <div class="radio">
            <input name="spec" id="still_radio" type="radio" value="static"
              v-model="sceneType"
            />
            <label for="still_radio" class="radio__name">Still</label>
          </div>
        </div>
      </div>

      <div id="stats" class="stats">
        <div class="stats__frames">
          <span class="frames__text">Number of frames</span>
          <input id="num-of-frames" type="text"
            v-model="framesCount"
          />
        </div>
        <div class="stats__avg-time">
          <span class="avg-time__text">Average render time per frame on your PC</span>
          <div class="inputs">
            <input id="hours" type="text" placeholder="Hours"
              v-model="hours"
            />
            <input id="minutes" type="text" placeholder="Minutes"
              v-model="minutes"
            />
          </div>
        </div>
      </div>
    </section>

    <section class="graphic-cards">
      <GraphicsCard
        v-model="cards"
        v-for="(card, index) in cards"
        :key="card.id"
        v-bind:cardInfo="card"
        v-bind:cardIndex="index"
        v-bind:cardsCount="cards.length"
        @updateCard="updateCard"
        @deleteCard="deleteCard"
        @addCard="addCard"
        @calculate="calculate"
      />
    </section>

    <section class="warning">
      <img src="../assets/images/warning.svg" alt="warning">
      <span class="warning__text">
        This estimation is approximate. It is based only on comparison of capacity between your PC and our servers.
        Try a free test to get more accurate estimation.
      </span>
    </section>

    <section class="result"
      v-if="isCalculated"
    >
      <div class="result__block">
        <div class="result__title-wrapper">
          <span class="result__title">Render time</span>
        </div>
        <div class="result__form">
          <span class="result__text">{{renderTime}}</span>
        </div>
      </div>
      <div class="result__block">
        <div class="result__title-wrapper">
          <span class="result__title">Price</span>
        </div>
        <div class="result__form">
          <span class="result__text">{{price}}</span>
        </div>
      </div>
    </section>
  </main>
</template>



<script>
  import GraphicsCard from "./GraphicsCard";

  export default {
    name: "Main",
    components: {GraphicsCard},
    data() {
      return {
        isCalculated: false,
        sceneType: 'static',
        framesCount: 1,
        hours: '',
        minutes: '',
        curId: 1,
        cards: [
          { id: 0, card: 'GT', line: '4xx', model: 'GT 420', count: 1 }
        ],
        renderTime: '',
        price: ''
      }
    },
    methods: {
      updateCard(data) {
        this.cards = this.cards.map(card => {
          if (card.id === data.id) return data
          return card
        })
      },
      deleteCard(index) {
        this.cards = this.cards.filter(card => card.id !== index)
      },
      addCard() {
        this.cards.push({ id: this.curId++, card: 'GT', line: '4xx', model: 'GT 420', count: 1 })
      },
      calculate() {
        const numOfFrames = document.getElementById('num-of-frames')
        const hours = document.getElementById('hours')
        const minutes = document.getElementById('minutes')
        const mainWindow = document.getElementById('main')
        const statsBlock = document.getElementById('stats')

        //0.25 server/hr or 0.25 hr on 1 server
        //0.31 AP

        if (this.framesCount && (this.hours || this.minutes)) {
          const benchmark = require('../data/gpu.json').benchmark

          const clientRenderTime = +this.hours*60 + +this.minutes
          let models = ''
          let clientBenchmark = 0
          this.cards.forEach(card => {
            models += `${card.model};`
            clientBenchmark += benchmark[card.model] * card.count
          })

          const renderTime = (clientBenchmark / 50 * clientRenderTime * this.framesCount).toFixed(2)
          const price = renderTime * 10
          this.renderTime = `${renderTime} server/hr or ${renderTime} hr on 1 server`
          this.price = `${price} AP`

          const info = {
            models,
            benchmark: clientBenchmark,
            rendererInfo: {
              sceneType: this.sceneType,
              framesCount: this.framesCount,
              renderTime: clientRenderTime
            }
          }
          console.log(info)

          this.isCalculated = true
          numOfFrames.classList.remove('not-filled')
          hours.classList.remove('not-filled')
          minutes.classList.remove('not-filled')
          setTimeout(() => { mainWindow.scrollTop = mainWindow.scrollHeight }, 0)
        }
        else {
          mainWindow.scrollTop = statsBlock.offsetHeight

          if (!this.framesCount) numOfFrames.classList.add('not-filled')
          else numOfFrames.classList.remove('not-filled')

          if (!(this.hours || this.minutes)) {
            hours.classList.add('not-filled')
            minutes.classList.add('not-filled')
          }
          else {
            hours.classList.remove('not-filled')
            minutes.classList.remove('not-filled')
          }
        }
      }
    },
    mounted() {

    }
  }
</script>









<style lang="scss" scoped>
  @import '~@/assets/scss/mixins.scss';

  .main {
    width: 100%;
    height: 97%;
    padding: 0 65px;
    margin-top: 15px;
    overflow-y: scroll;
  }

  .render {
    display: flex;
    flex-direction: column;
    min-height: 144px;
    background-color: #F7F8FC;
    border-radius: 4px;
    padding: 8px 21px;

    &__title span {
      font-weight: 500;
      line-height: 20px;
    }

    &__image {
      flex-grow: 1;
      height: 100%;
      display: flex;
      justify-content: center;
    }
  }

  .spec {
    position: relative;

    .order-pic {
      position: absolute;
      left: -45px;
      top: 170px;
    }
  }

  .details {
    display: flex;
    flex-direction: column;
    position: relative;
    min-height: 109px;
    background-color: #F7F8FC;
    border-radius: 4px;
    margin-top: 7px;
    padding: 21px 21px;

    &__title {
      line-height: 20px;
      margin-bottom: 25px;
      font-weight: 500;
    }

    &::before {
      content: '';
      display: block;
      position: absolute;
      width: 18px;
      height: 44px;
      background-image: url("../assets/images/triangle.svg");
      left: -13px;
      top: 8px;
    }
  }

  .radios {
    display: flex;

    .radio:not(:first-child) {
      margin-left: 100px;
    }
  }

  .radio {
    display: flex;
    align-items: center;

    label {
      position: relative;
      font-size: 14px;
      line-height: 18px;
      color: #68677B;
    }

    input+label::after {
      content: '';
      display: block;
      width: 18px;
      height: 18px;
      background-color: white;
      border: 2px solid rgba(28, 92, 237, 0.1);
      border-radius: 50%;
      position: absolute;
      top: 50%;
      right: -25px;
      transform: translate(0, -50%);
    }

    input:checked+label::before {
      content: '';
      display: inline-block;
      z-index: 1;
      width: 10px;
      height: 10px;
      border-radius: 50%;
      position: absolute;
      right: -21px;
      top: 50%;
      transform: translate(0, -50%);
      background-color: #526AE5;
    }

    input {
      position: absolute;
      z-index: -1;
      opacity: 0;
    }
  }

  .stats {
    display: flex;
    position: relative;
    min-height: 109px;
    background-color: #F7F8FC;
    border-radius: 4px;
    margin-top: 7px;
    padding: 21px 21px;

    &::before {
      content: '';
      display: block;
      position: absolute;
      width: 18px;
      height: 44px;
      background-image: url("../assets/images/triangle.svg");
      left: -13px;
      top: 8px;
    }

    &__frames {
      display: flex;
      flex-direction: column;
      margin-right: 114px;

      .frames__text {
        color: #68677B;
        font-size: 14px;
      }
    }

    &__avg-time {
      display: flex;
      flex-direction: column;

      .avg-time__text {
        color: #68677B;
        font-size: 14px;
      }

      input:first-child {
        margin-right: 25px;
      }
    }

    input {
      @include input-mixin;
      margin-top: 10px;
    }

    .not-filled {
      border: 1px solid red;
    }
  }

  .graphic-cards {
    margin-top: 7px;
    position: relative;

    &::before {
      content: '';
      display: block;
      position: absolute;
      width: 18px;
      height: 44px;
      background-image: url("../assets/images/triangle.svg");
      left: -13px;
      top: 8px;
    }
  }

  .warning {
    display: flex;
    align-items: center;
    min-height: 85px;
    background-color: #F7F8FC;
    border-radius: 4px;
    margin-top: 7px;
    padding: 22px 28px;

    &__text {
      font-family: Poppins, sans-serif;
      font-style: normal;
      font-weight: 300;
      font-size: 14px;
      line-height: 21px;
      color: #FF5F74;
      margin-left: 17px;
    }
  }

  .result {
    display: flex;
    flex-direction: column;
    min-height: 204px;
    background-color: #F7F8FC;
    border-radius: 4px;
    margin-top: 7px;
    padding: 50px 20px;

    &__block {
      position: relative;
      display: flex;
      justify-content: center;
      align-items: center;
      margin-bottom: 11px;
    }

    &__title-wrapper {
      position: absolute;
      left: 0;
      width: 100px;
    }

    &__title {
      line-height: 20px;
      color: #526AE5;
    }

    &__form {
      width: 336px;
      height: 45px;
      background-color: white;

      border: 1px solid #DFE2E6;
      border-radius: 4px;
      padding: 13px;
    }

    &__text {
      font-size: 14px;
      line-height: 18px;
      color: #A7ABC3;
    }
  }

  ::-webkit-scrollbar {
    width: 6px;
    background-color: inherit;
  }

  ::-webkit-scrollbar-thumb {
    background: rgba(196, 196, 196, 0.8);
    border-radius: 3px;
  }

  ::-webkit-scrollbar-thumb:hover {
    background: rgba(196, 196, 196, 1);
  }

  @media (max-width: 768px) {
    .main {
      overflow-y: auto;
      padding: 15px;
      margin: 0;
    }

    .order-pic {
      display: none;
    }

    .details {

      &::before {
        display: none;
      }
    }

    .stats {

      &::before {
        display: none;
      }
    }

    .graphic-cards {
      margin-top: 0;

      &::before {
        display: none;
      }
    }

    .spec {
      background-color: white;
      border-radius: 4px;

      .render {
        background-color: white;
        padding: 0;
        margin: 0;
        border-radius: 4px 4px 0 0;
        align-items: center;

        &__title {
          margin: 21px 0 3px 0;
        }

        &__image {
          min-width: 138px;
          min-height: 106px;
          background: #FFFFFF;
          opacity: 0.9;
          box-shadow: 0 4px 50px rgba(28, 92, 237, 0.1);
          border-radius: 9px;
          flex-grow: 0;
          align-items: center;
          margin-bottom: 60px;
        }
      }

      .details {
        background-color: white;
        padding: 0;
        margin: 0 27px;
        border-radius: 0;
        border-bottom: 1px solid rgba(28, 92, 237, 0.1);
        align-items: center;
        min-height: 0;

        &__title {
          margin-bottom: 9px;
        }

        .radios {
          flex-direction: column;
          margin-bottom: 16px;
          padding-right: 65px;

          .radio {
            flex-direction: row-reverse;
          }

          .radio:not(:first-child) {
            margin-left: 0;
            margin-top: 20px;
          }
        }
      }

      .stats {
        background-color: white;
        padding: 0;
        margin: 0;
        border-radius: 0 0 4px 4px;

        flex-direction: column;
        align-items: center;

        &__frames {
          margin: 0;

          .frames__text {
            text-align: center;
            margin-top: 19px;
          }

          input {
            margin-top: 16px;
            margin-bottom: 21px;
          }
        }

        &__avg-time {

          .inputs {
            display: flex;
            flex-direction: column;
            align-items: center;

            input {
              margin: 0;
            }

            input:not(:last-child) {
              margin-bottom: 16px;
            }

            input:last-child {
              margin-bottom: 39px;
            }
          }

          .avg-time__text {
            margin-bottom: 19px;
          }
        }
      }
    }

    .warning {
      background: #FFFFFF;
      padding: 15px 10px;

      &__text {
        font-weight: 300;
        font-size: 12px;
        line-height: 131.7%;
        color: #FF5F74;
      }
    }

    .result {
      background: #FFFFFF;
      padding: 35px 8px;

      &__title-wrapper {
        position: relative;
        margin: 0 0 11px 13px;
      }

      &__block {
        flex-direction: column;
        align-items: start;
      }

      &__form {
        width: 100%;
      }
    }
  }
</style>