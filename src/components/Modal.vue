<template>
    <div class="overlay">
        <div class="modal">
            <button class="modal-close" @click="$emit('hide')"></button>

            <h1 class="modal-title">Налоговый вычет</h1>
            <p class="modal-description">Используйте налоговый вычет чтобы погасить ипотеку досрочно. Размер налогового
                вычета составляет не более 13% от своего официального годового дохода.</p>

            <div class="modal-input">
                <p class="modal-input__title">Ваша зарплата в месяц</p>

                <input 
                class="modal-input__input" 
                :class="{ 'error-input': error }" 
                type="number" 
                placeholder="Введите данные" 
                @input="inputValue = $event.target.value" 
                @focus="error = false">

                <transition name="fade">
                    <div v-if="error" class="modal-input__error">Поле обязательно для заполнения</div>
                </transition>

                <button class="modal-input__btn" @click="paymentsCalc(inputValue)">Рассчитать</button>
            </div>

            <transition name="fade">
                <div v-if="paymentsList.length > 0" class="payments-list">
                    <div class="payments-list__title">Итого можете внести в качестве досрочных:</div>
                    <div 
                    v-for="(item, index) in paymentsList" 
                    :key="item.yearName" 
                    class="payments-list__item">
                        <div class="payments-item__checkbox">
                            <input type="checkbox" :id="index">
                            <label :for="index"></label>
                        </div>
                        <div class="payments-item__text">
                            <span>{{ item.yearSum }} {{ currency }}</span> {{ item.yearName }}
                        </div>
                    </div>
                </div>
            </transition>

            <div class="result">
                <p class="result-title">Что уменьшаем?</p>

                <div class="result-btn">
                    <input type="radio" id="payments" name="result" value="payments" checked>
                    <label for="payments">Платёж</label>
                </div>
                <div class="result-btn">
                    <input type="radio" id="time" name="result" value="time">
                    <label for="time">Срок</label>
                </div>
            </div>

            <button class="btn-gradient">Досрочный платёж</button>
        </div>
    </div>
</template>

<script>
export default {
  name: 'Modal',
  data() {
    return {
      titles: [
        {yearName: 'в 1-ый год',yearSum: 0},
        {yearName: 'во 2-ой год',yearSum: 0},
        {yearName: 'в 3-ий год',yearSum: 0},
        {yearName: 'в 4-ый год',yearSum: 0},
        {yearName: 'в 5-ый год',yearSum: 0},
        {yearName: 'в 6-ой год',yearSum: 0},
        {yearName: 'в 7-ой год',yearSum: 0},
        {yearName: 'в 8-ой год',yearSum: 0},
        {yearName: 'в 9-ый год',yearSum: 0},
        {yearName: 'в 10-ый год',yearSum: 0},
        {yearName: 'в 11-ый год',yearSum: 0},
        {yearName: 'в 12-ый год',yearSum: 0},
        {yearName: 'в 13-ый год',yearSum: 0},
        {yearName: 'в 14-ый год',yearSum: 0},
        {yearName: 'в 15-ый год',yearSum: 0},
        {yearName: 'в 16-ый год',yearSum: 0},
        {yearName: 'в 17-ый год',yearSum: 0}
      ],
      inputValue: 0,
      paymentsList: [],
      currency: 'рублей',
      error: false
    }
  },
  methods: {
    paymentsCalc(val) {
      if (val) {
        this.error = false;

        let cashBack = 260000;

        let yearCashback = (val * 12) * 0.13;
        
        let fullCashbackQuantity = Math.floor(cashBack / yearCashback);

        let lastYear = cashBack % yearCashback;

        let userCashback = this.titles.slice(0, fullCashbackQuantity + 1);

        for(let i = 0; i < userCashback.length; i++) {

          if (i !== userCashback.length - 1) {
              userCashback[i].yearSum = yearCashback.toFixed(0);
          } else {
              userCashback[i].yearSum = lastYear.toFixed(0); 
          }

        }

        return this.paymentsList = userCashback;
      } else {
        this.error = true;
      }
    }
  }
}
</script>

<style lang="scss">
.overlay {
  position: relative;

  width: 100%;
  min-height: 100%;
  padding: 120px 158px 50px;

  background: #b3b3b3;
}

.modal {
  position: relative;

  max-width: 552px;
  margin: 0 auto;
  padding: 2rem;

  background: #ffffff;
  border-radius: 30px;
}

.modal-close {
  position: absolute;
  top: 1rem;
  right: 1rem;

  display: block;
  width: 2.5rem;
  height: 2.5rem;
  padding: 0;

  background: transparent url(../assets/images/cross.svg) no-repeat center;
  background-size: 18px;
  border: none;
  outline: none;
  cursor: pointer;
}

.modal-title {
  margin: 0 0 1rem;

  font-weight: 500;
  font-size: 28px;
  line-height: 2.5rem;
}

.modal-description {
  margin: 0 0 1.5rem;

  font-size: 14px;
  line-height: 1.5rem;
  color: #808080;
}

.modal-input {
  margin-bottom: 1rem;

  .modal-input__title {
    margin: 0 0 0.5rem;

    font-weight: 500;
    font-size: 14px;
    line-height: 1.5rem;
  }

  .modal-input__input {
    display: block;
    width: 100%;
    height: 2.5rem;
    margin: 0 0 0.5rem;
    padding: 0 10px;

    font-size: 14px;
    line-height: 100%;
    font-family: inherit;
    color: #000000;

    border: 1px solid #dfe3e6;
    border-radius: 3px;
    outline: none;

    transition: 0.3s;

    appearance: none;
  }

  .modal-input__input::placeholder {
    font-size: 14px;
    color: #bec5cc;
  }

  .modal-input__input:hover {
    border: 1px solid #000000;
  }

  .modal-input__input:focus {
    border: 1px solid #dfe3e6;
  }

  .error-input {
    margin: 0 0 4px;

    border: 1px solid #ea0029;
  }

  .modal-input__input::-webkit-outer-spin-button,
  .modal-input__input::-webkit-inner-spin-button {
    margin: 0; /* <-- Apparently some margin are still there even though it's hidden */
    /* display: none; <- Crashes Chrome on hover */

    -webkit-appearance: none;
  }

  .modal-input__error {
    margin: 0 0 0.5rem;

    font-size: 10px;
    line-height: 12px;
    color: #ea0029;

    transition: 0.5s;
  }

  .modal-input__btn {
    padding: 0;

    font-weight: 500;
    font-size: 14px;
    line-height: 1.5rem;
    font-family: inherit;
    color: #ea0029;

    background: transparent;
    border: 0;
    outline: none;
    cursor: pointer;

    transition: 0.3s;
  }

  .modal-input__btn:hover {
    color: #f53a31;
  }

  .modal-input__btn:active {
    color: #ea0029;
  }
}

.payments-list {
  margin: 1rem 0;

  .payments-list__title {
    margin-bottom: 1rem;

    font-weight: 500;
    font-size: 14px;
    line-height: 1.5rem;
  }

  .payments-list__item {
    display: flex;
    align-items: center;
    padding: 1rem 0;

    border-bottom: 1px solid #dfe3e6;
  }

  .payments-item__checkbox {
    margin-right: 11px;

    input {
      display: none;
    }

    input + label {
      position: relative;

      display: block;
      width: 20px;
      height: 20px;

      background: #ffffff;
      border: 1px solid #dfe3e6;
      border-radius: 6px;
      cursor: pointer;

      transition: 0.3s;
    }

    input + label:hover {
      border: 1px solid #000000;
    }

    input:checked + label {
      background: linear-gradient(255.35deg, #dc3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #ff5e56;
      border: none;
    }

    input:checked + label:before {
      content: "";
      top: 0;
      left: 0;

      display: block;
      width: 100%;
      height: 100%;

      background: transparent url(/img/checkbox_arrow.3c6aa00f.svg) no-repeat center;
      background-size: 14px 11px;
    }
  }

  .payments-item__text {
    font-size: 14px;
    line-height: 1.5rem;
    color: #808080;

    span {
      color: #000000;
    }
  }
}

.result {
  display: flex;
  align-items: center;
  margin: 0 0 2.5rem;

  .result-title {
    margin-right: 2rem;

    font-weight: 500;
    font-size: 14px;
    line-height: 1.5rem;
  }

  .result-btn {
    margin-right: 1rem;

    &:last-of-type {
      margin-right: 0;
    }

    input {
      display: none;
    }

    input + label {
      display: block;
      padding: 6px 12px;

      font-size: 14px;
      line-height: 1.5rem;

      background: #eef0f2;
      border: none;
      border-radius: 50px;
      outline: none;
      cursor: pointer;

      transition: 0.3s;
    }


    input:checked + label {
      color: #ffffff;

      background: linear-gradient(255.35deg, #dc3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #ff5e56;
    }
  }
}

.btn-gradient {
  display: block;
  width: 100%;
  height: 56px;
  padding: 0 10px;

  font-weight: 500;
  font-size: 16px;
  line-height: 100%;
  font-family: inherit;
  text-align: center;
  color: #ffffff;

  background: linear-gradient(255.35deg, #dc3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #ff5e56;
  border: none;
  border-radius: 6px;
  outline: none;
  box-shadow: 0 0 24px rgba(234, 0, 41, 0.33);
  box-shadow: none;
  cursor: pointer;

  transition: 0.3s;

  &:hover,
  &:active {
    background: #ea0029;
  }

  &:disabled,
  &:disabled:hover {
    background: #bec5cc;
  }
}

@media screen and (max-width: 768px) {
  .overlay {
    width: 100%;
    height: 100%;
    min-height: 100%;
    padding: 0;
  }

  .modal {
    max-width: 100%;
    min-height: 100%;
    margin: 0 auto;
    padding: 2rem 1rem calc(2rem + 56px);

    border-radius: 0;
  }

  .modal-title {
    font-size: 18px;
    line-height: 1.5rem;
  }

  .btn-gradient {
    position: absolute;
    bottom: 2rem;
    left: 50%;

    width: calc(100% - 2rem);

    transform: translateX(-50%);
  }

  .modal-close {
    width: 1.5rem;
    height: 1.5rem;

    background-size: 12px;
  }
}

</style>