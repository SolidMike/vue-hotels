<template>
  <div id="app">
    <div class="wrapper">
      <aside class="filter">
        <label for="country"
          >Страна
          <v-select
            multiple
            v-model="selected_countries"
            :options="countries"
            id="country"
          ></v-select>
        </label>
        <label for="type"
          >Тип
          <v-select
            multiple
            v-model="selected_types"
            :options="types"
          ></v-select>
        </label>
        <fieldset>
          <legend>Количество звезд</legend>
          <label for="one_star"
            >1 звезда
            <input
              type="checkbox"
              name="rating"
              id="one_star"
              value="1"
              v-model="checked_stars"
            />
          </label>
          <label for="two_stars"
            >2 звезды
            <input
              type="checkbox"
              name="rating"
              id="two_stars"
              value="2"
              v-model="checked_stars"
            />
          </label>
          <label for="three_stars"
            >3 звезды
            <input
              type="checkbox"
              name="rating"
              id="three_stars"
              value="3"
              v-model="checked_stars"
            />
          </label>
          <label for="four_stars"
            >4 звезды
            <input
              type="checkbox"
              name="rating"
              id="four_stars"
              value="4"
              v-model="checked_stars"
            />
          </label>
          <label for="five_stars"
            >5 звезд
            <input
              type="checkbox"
              name="rating"
              id="five_stars"
              value="5"
              v-model="checked_stars"
            />
          </label>
        </fieldset>
        <label for="reviews"
          >Кол-во отзывов (от)
          <input
            @keypress="isNumber($event)"
            type="text"
            v-model="reviews_amount"
            id="reviews"
          />
        </label>
        <label for="vol">Цена (от 0 до 5000):</label>
        <!-- <input
          type="range"
          v-model="price_range"
          min="0"
          max="5000"
          step="10"
        /> -->
        <vue-slider
          ref="slider"
          v-model="price_range"
          v-bind="priceRangeOptions"
          class="price-slider"
        ></vue-slider>
        <output
          class="price-slider__output"
          v-text="'Цента от:' + priceOutput[0]"
        ></output>
        <output
          class="price-slider__output"
          v-text="'Цента до:' + priceOutput[1]"
        ></output>
        <!-- Если делать так и передавать в <HotlesList v-bind:listData="hotels"> перестает работать filtersReset. Пока не понял почему. -->
        <!-- <button type="button" @click="filtersApply(filteredHotels)">
          Применить фильтры
        </button> -->
        <button type="button" @click="filtersReset">Очистить фильтры</button>
      </aside>
      <main>
        <HotelsList v-bind:listData="filteredHotels" />
      </main>
    </div>
  </div>
</template>

<script>
import vSelect from 'vue-select'
import 'vue-select/dist/vue-select.css'
import VueSlider from 'vue-slider-component'
import 'vue-slider-component/theme/default.css'
import HotelsList from './components/HotelsList'

export default {
  name: 'App',
  data() {
    return {
      hotels: [
        {
          name: 'Marina Inn',
          country: 'Австрия',
          address: 'Фалираки, Родос, Греция',
          stars: 4,
          type: 'Отель',
          description:
            'Этот 4-звездочный отель расположен в центре города. На его территории есть бассейн с террасой и сауна. Из номеров открывается вид на море или на средневековый город.',
          services: [
            'Пляж',
            'Кондиционер',
            'Открытый бассейн',
            'Бесплатная парковка',
            'Бесплатный WiFi',
            'Вид на море',
            'Бесплатный завтрак',
          ],
          min_price: 2789.0,
          currency: 'RUR',
          rating: 4.5,
          reviews_amount: 18,
          last_review:
            'Отличное расположение. Вкусный завтрак. Отдыхали с детьми - все понравилось.',
        },
        {
          name: 'Mondrian Suites',
          country: 'Греция',
          address: 'Фалираки, Родос, Греция',
          stars: 5,
          type: 'Апартаменты',
          description:
            'Из окон открывается вид на город.К услугам гостей номера-студио с балконом и чайником в 2,7 км от пляжа.',
          services: [
            'Пляж',
            'Кондиционер',
            'Открытый бассейн',
            'Платная парковка',
            'Бесплатный WiFi',
            'Вид на море',
          ],
          min_price: 3245.2,
          currency: 'RUR',
          rating: 5,
          reviews_amount: 4,
          last_review:
            'Потрясающее место, в номере есть все необходимое. Красивый вид на море.',
        },
        {
          name: 'Sunny Appartments',
          country: 'Греция',
          address: 'Родос, Родос, Греция',
          stars: 2,
          type: 'Апартаменты',
          description:
            'Все номера и апартаменты оборудованы кондиционером и телевизором с плоским экраном. Также в распоряжении гостей тостер и чайник.',
          services: [
            'Пляж',
            'Кондиционер',
            'Бесплатная парковка',
            'Бесплатный WiFi',
          ],
          min_price: 1153.0,
          currency: 'RUR',
          rating: 2.4,
          reviews_amount: 36,
          last_review:
            'Бассейн очень маленький. Кормят невкусно. Больше не поедем.',
        },
        {
          name: 'Super All Inclusive Hotel',
          country: 'Греция',
          address: 'Родос, Родос, Греция',
          stars: 4,
          type: 'Отель',
          description:
            'Все номера оснащены телевизором с плоским экраном. Из некоторых номеров открывается вид на море или город.',
          services: [
            'Пляж',
            'Кондиционер',
            'Открытый бассейн',
            'Бесплатный WiFi',
            'Вид на море',
            'Бесплатный завтрак',
          ],
          min_price: 3689.0,
          currency: 'RUR',
          rating: 4.1,
          reviews_amount: 14,
          last_review:
            'Недалёко от пляжа и старого города, вокруг много разных магазинчиков',
        },
        {
          name: 'Adams Hotel',
          country: 'Греция',
          address: 'Родос, Родос, Греция',
          stars: 3,
          type: 'Отель',
          description:
            'Отель расположен всего в 100 метрах от пляжа и в 5-ти минутах ходьбы от исторической части города, недалеко от всех основных достопримечательностей. Из отеля открывается вид на море. К услугам гостей спокойный открытый бассейн.',
          services: [
            'Пляж',
            'Кондиционер',
            'Открытый бассейн',
            'Бесплатная парковка',
            'Бесплатный WiFi',
            'Бесплатный завтрак',
          ],
          min_price: 1896.0,
          currency: 'RUR',
          rating: 0,
          reviews_amount: 0,
          last_review: '',
        },
      ],
      selected_types: null,
      types: ['Апартаменты', 'Отель'],
      selected_countries: null,
      countries: ['Австрия', 'Албания', 'Греция'],
      reviews_amount: '',
      checked_stars: [],
      price_range: [0, 5000],
      priceRangeOptions: {
        enableCross: false,
        min: 0,
        max: 5000,
        tooltip: 'active',
        tooltipPlacement: 'bottom',
      },
    }
  },
  //   watch: {
  //     price_range(value) {
  //       console.dir(value)
  //     },
  //   },
  computed: {
    filteredHotels() {
      return this.filteredByCountry(
        this.filteredByType(
          this.filteredByReviewsAmount(
            this.filteredByStars(this.filteredByPrice(this.hotels))
          )
        )
      )
    },
    priceOutput() {
      return this.price_range
    },
  },
  methods: {
    isNumber(evt) {
      evt = evt ? evt : window.event
      var charCode = evt.which ? evt.which : evt.keyCode
      if (
        charCode > 31 &&
        (charCode < 48 || charCode > 57) &&
        charCode !== 46
      ) {
        evt.preventDefault()
      } else {
        return true
      }
    },
    filteredByType(hotels) {
      if (this.selected_types == null || !this.selected_types.length)
        return hotels
      return hotels.filter((h) => this.selected_types.includes(h.type))
    },
    filteredByCountry(hotels) {
      if (this.selected_countries == null || !this.selected_countries.length)
        return hotels
      return hotels.filter((h) => this.selected_countries.includes(h.country))
    },
    filteredByReviewsAmount(hotels) {
      if (!this.reviews_amount) return hotels
      return hotels.filter((h) => this.reviews_amount <= h.reviews_amount)
    },
    filteredByStars(hotels) {
      if (!this.checked_stars.length) return hotels
      return hotels.filter((h) =>
        this.checked_stars.includes(h.stars.toString())
      )
    },
    filteredByPrice(hotels) {
      if (!this.price_range.length) return hotels
      return hotels.filter(
        (h) =>
          this.price_range[0] <= h.min_price &&
          this.price_range[1] >= h.min_price
      )
    },
    // filtersApply(array) {
    //   this.hotels = array
    // },
    filtersReset() {
      this.selected_countries = null
      this.selected_types = null
      this.reviews_amount = ''
      this.checked_stars = []
      this.price_range = [0, 5000]
    },
  },
  components: {
    HotelsList,
    VueSlider,
    vSelect,
  },
}
</script>

<style>
ul[class],
ol[class] {
  list-style: none;
  padding: 0;
  margin: 0;
}

.wrapper {
  display: grid;
  grid-template-columns: minmax(150px, 325px) 1fr;
  grid-gap: 2rem;
  padding: 0 1rem;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}

.price-slider__output {
  opacity: 0;
  transition: opacity 0.3s;
}
.price-slider:hover ~ .price-slider__output {
  opacity: 1;
}
.filter {
  display: flex;
  flex-direction: column;
}
.filter > *:not(:first-child) {
  margin-top: 1rem;
}
fieldset {
  display: flex;
  flex-direction: column;
}
</style>
