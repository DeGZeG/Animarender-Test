<template>
  <div class="card">
    <span class="card__title"
      v-if="cardIndex === 0"
    >Choose your PC specs</span>

    <div class="card__inputs">
      <div class="card__input">
        <label for="graphics-card" class="input__title">Graphics card</label>
        <select class="card__select" name="graphics-card" id="graphics-card"
          @change="onChangeOption($event, 'curCard')"
          v-model="curCard"
        >
          <option
            v-for="(value, card) in gpuInfo"
            v-bind:value='card'
          >
            {{card}}
          </option>
        </select>
      </div>
      <div class="card__input">
        <label for="line" class="input__title">Graphics card line</label>
        <select class="card__select" name="line" id="line"
          @change="onChangeOption($event, 'curLine')"
          v-model="curLine"
        >
          <option
            v-for="(value, line) in gpuInfo[curCard]"
            v-bind:value='line'
          >
            {{line}}
          </option>
        </select>
      </div>
      <div class="card__input">
        <label for="model" class="input__title">Graphics card model</label>
        <select class="card__select model__select" name="model" id="model"
          @change="onChangeOption($event, 'curModel')"
          v-model="curModel"
        >
          <option
            v-for="model in gpuInfo[curCard][curLine]"
            v-bind:value='model'
          >
            {{model}}
          </option>
        </select>
      </div>
      <div class="card__input">
        <label for="amount" class="input__title">Number of cards</label>
        <select class="card__select" name="amount" id="amount"
          @change="onChangeOption($event)"
          v-model="curCount"
        >
          <option
            v-for='index in 10'
            v-bind:value='index'
          >
            {{index}}
          </option>
        </select>
      </div>
      <img class="card__remove" src="../assets/images/minus.svg" alt="remove card"
        @click="deleteCard"
        v-if="cardsCount > 1"
      >
    </div>

    <img class="card__remove-mobile" src="../assets/images/close.svg" alt="remove card"
      @click="deleteCard"
      v-if="cardsCount > 1"
    >

    <div class="card__menu"
      v-if="cardIndex === cardsCount-1"
    >
      <div class="menu__add">
        <span class="add__text">Add another type card</span>
        <img class="add__button" src="../assets/images/plus.svg" alt="add card"
          @click="addCard"
        >
      </div>
      <div class="menu__calc">
        <button class="calc-button"
          @click="calculate"
        >Calculate</button>
      </div>
    </div>
  </div>
</template>

<script>
  export default {
    name: "GraphicCard",
    data() {
      return {
        gpuInfo: require('../data/gpu.json').list,
        curCard: this.cardInfo.card,
        curLine: this.cardInfo.line,
        curModel: this.cardInfo.model,
        curCount: this.cardInfo.count,
      }
    },
    props: ['cardInfo', 'cardIndex', 'cardsCount'],
    methods: {
      onChangeOption(event, field) {
        this[field] = event.target.value
        if (field === 'curCard') {
          let counter = 0;
          for (let line in this.gpuInfo[this.curCard]) {
            if (counter === 0) this.curLine = line
            counter++
          }

          counter = 0;
          for (let model of this.gpuInfo[this.curCard][this.curLine]) {
            if (counter === 0) this.curModel = model
            counter++
          }
        }

        else if (field === 'curLine') {
          let counter = 0;
          for (let model of this.gpuInfo[this.curCard][this.curLine]) {
            if (counter === 0) this.curModel = model
            counter++
          }
        }

        this.updateCard()
      },
      updateCard() {
        this.$emit(
          'updateCard',
          {
            id: this.cardInfo.id,
            card: this.curCard,
            line: this.curLine,
            model: this.curModel,
            count: this.curCount
          }
        )
      },
      addCard() {
        this.$emit('addCard')
      },
      deleteCard() {
        this.$emit(
          'deleteCard',
          this.cardInfo.id
        )
      },
      calculate() {
        this.$emit('calculate')
      }
    },
  }
</script>










<style lang="scss" scoped>
  @import '~@/assets/scss/mixins.scss';

  .card {
    display: flex;
    flex-direction: column;
    position: relative;
    min-height: 109px;
    background-color: #F7F8FC;
    border-radius: 4px;
    padding: 21px 21px;

    &__title {
      font-weight: 500;
      line-height: 20px;
      margin-bottom: 14px;
    }

    &__inputs {
      display: flex;
      flex-wrap: wrap;
    }

    &__input {
      display: flex;
      flex-direction: column;
      margin-right: 24px;
    }

    .input__title {
      color: #68677B;
      margin-bottom: 7px;
    }

    &__select {
      @include input-mixin;
      appearance: none;
      position: relative;
      background-image: url("../assets/images/arrow.svg");
      background-repeat: no-repeat;
      background-position: 90% 50%;
    }

    .model__select {
      width: 180px;
    }

    &__remove {
      padding-top: 25px;

      &:hover {
        cursor: pointer;
      }
    }

    &__remove-mobile {
      position: absolute;
      top: -5px;
      right: -10px;
      cursor: pointer;
      display: none;
    }
  }

  .card__menu {
    display: flex;
    flex-direction: column;
    margin-top: 28px;

    .menu__add {
      display: flex;
      align-items: center;

      .add__text {
        font-size: 14px;
        line-height: 18px;
        color: #68677B;
        margin-right: 13px;
      }
      
      .add__button {
        cursor: pointer;
      }
    }

    .menu__calc {
      display: flex;
      justify-content: center;
      margin: 24px 0 16px 0;

      .calc-button {
        width: 179px;
        height: 40px;
        background: #526AE5;
        border: 1px solid rgba(206, 208, 214, 0.3);
        box-sizing: border-box;
        box-shadow: -5px -5px 10px #FAFBFF, 5px 5px 10px #A6ABBD;
        border-radius: 46px;
        cursor: pointer;

        color: white;
        font-family: Poppins, sans-serif;
        font-style: normal;
        font-weight: bold;
        font-size: 0.75rem;
        line-height: 18px;

        &:focus {
          outline: none;
        }

        transition: 0.3s transform;
        &:hover {
          transform: scale(1.03);
        }
      }
    }
  }

  @media (max-width: 768px) {
    .card {
      margin-top: 7px;
      background-color: white;

      &__title {
        text-align: center;
      }

      &__inputs {
        flex-direction: column;
        align-items: center;
      }

      &__input {
        margin: 0 0 15px 0;
      }

      &__remove {
        display: none;
      }

      &__remove-mobile {
        display: block;
      }

      &__menu {

        .menu__add {
          justify-content: center;
        }
      }
    }
  }
</style>