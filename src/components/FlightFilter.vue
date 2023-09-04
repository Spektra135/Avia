<template>
  <aside class="filter">
    <form class="filter__form form" action="#" method="get" @submit.prevent="submit">
      <fieldset class="form__block">
        <legend class="form__legend">Сортировать</legend>

        <ul class="colors">
          <li class="colors__item" v-for="sorting in sortings" :key="sorting.value">
            <label class="colors__label">
              <input class="" type="radio" name="sorting" :value="sorting.value" v-model="currentSorting" >

              <span> {{ sorting.name }}</span>
            </label>
          </li>
        </ul>
      </fieldset>

      <fieldset class="form__block">
        <legend class="form__legend">Фильтровать</legend>

        <ul class="check-list">
          <li class="check-list__item" v-for="transfer in transfers" :key="transfer.value">
            <label class="check-list__label">
              <input  type="checkbox" name="transfer" :value="transfer.value" v-model="currentTransfer">

              <span>{{ transfer.name }}</span>
            </label>
          </li>
        </ul>
      </fieldset>

      <fieldset class="form__block">
        <legend class="form__legend">Цена</legend>

        <label class="form__label form__label--price">
          <span class="form__input-value">От</span>

          <input class="form__input" type="text" name="min-price" v-model.number="currentPriceFrom" placeholder="0">
        </label>

        <label class="form__label form__label--price">
          <span class="form__input-value">До</span>

          <input class="form__input" type="text" name="max-price" v-model.number="currentPriceTo" placeholder="1000000">
        </label>
      </fieldset>

      <fieldset class="form__block">
        <legend class="form__legend">Авиакомпании</legend>

        <ul class="check-list">
          <li class="check-list__item" v-for="carrier in carriers" :key="carrier">
            <label class="check-list__label">
              <input  type="checkbox" name="carrier" :value="carrier" v-model="currentCarrier">
              <span>
                 {{ carrier }}
              </span>
            </label>
          </li>
        </ul>
      </fieldset>

      <button class="filter__submit button button--primery" type="submit">
        Применить
      </button>
      <button class="filter__reset button button--second" type="button" @click="reset">
        Сбросить
      </button>
    </form>
  </aside>
</template>

<script>
import dataFlights from "@/data/flights.json";
  export default {
    data() {
      return {
        currentPriceFrom: '',
        currentPriceTo: '',

        currentFlightTime: 0,
        currentTransfer: [],
        currentCarrier: [],

        currentSorting: '',
      }
    },
    props: ['priceFrom', 'priceTo', 'carrier', 'transfer', 'sorting'],
    computed: {
      sortings() {
        return [
          {
            value: "minPrice",
            name:  " - по возрастанию цены"
          },
          {
            value: "maxPrice",
            name: " - по убыванию цены"
          },
          {
            value: "minTime",
            name: " - по времени в пути"
          },
        ]
      },
      carriers() {
        if (!dataFlights.result.flights) return [];
        let set = new Set();
        for (let flight of dataFlights.result.flights) {
          set.add(flight.flight.carrier.caption);
        }
        return set;
      },
      transfers() {
        return [
          {
            value: 1,
            name: " - 1 пересадка"
          },
          {
            value: 0,
            name: " - без пересадок"
          },
        ]
      },
    },
    watch: {
      priceFrom(value) {
        if (this.currentPriceFrom < 0) {
          this.currentPriceFrom = 0;
        } else {
          this.currentPriceFrom = value;
        }
      },
      priceTo(value) {
        if (this.currentPriceTo  < 0) {
          this.currentPriceFrom = 0;
        } else {
          this.currentPriceTo = value;
        }
      },
      carrier(checked) {
        this.currentCarrier = checked;
      },
      transfer(checked) {
        this.currentTransfer = checked;
      },
      sorting(value) {
        this.currentSorting = value;
      },
    },
    methods: {
      submit() {
        this.$emit('update:priceFrom', this.currentPriceFrom)
        this.$emit('update:priceTo', this.currentPriceTo)
        this.$emit('update:carrier', this.currentCarrier)
        this.$emit('update:transfer', this.currentTransfer)
        this.$emit('update:sorting', this.currentSorting)
      },
      // Добавить обнуление выбора пересадки и авиаперевозчика
      reset() {
        this.$emit('update:priceFrom', '')
        this.$emit('update:priceTo', '')
        /*this.$emit('update:categoryId', 0)
        this.$emit('update:colorId', 0)*/
      },
    },
  }
</script>
