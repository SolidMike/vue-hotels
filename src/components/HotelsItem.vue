<template>
  <li class="grid__item">
    <article class="hotel">
      <div class="hotel__inner">
        <div class="hotel__heading">
          <div class="hotel__title">{{ hotel.name }}</div>
          <div class="hotel__price">
            {{ hotel.min_price }}<br />
            Цена за 1 ночь
          </div>
        </div>
        <div class="hotel__info">
          <ul class="hotel__stars rating">
            <li class="rating__star" v-for="(star, i) in hotel.stars" :key="i">
              <div>&#9733;</div>
            </li>
          </ul>
          <div class="hotel__type">{{ hotel.type }}</div>
          <div class="hotel__reviews">{{ hotel.reviews_amount }}</div>
          <div class="hotel__country">{{ hotel.country }}</div>
        </div>
        <div class="hotel__body">
          <div class="hotel__descr">{{ hotel.description }}</div>
          <div class="hotel__booked"></div>
        </div>
      </div>
    </article>
  </li>
</template>

<script>
export default {
  props: {
    hotel: {
      type: Object,
      required: true,
    },
  },
}
</script>

<style>
.grid__item:not(:first-child) {
  margin-top: 1.5rem;
}

.hotel {
  padding: 2rem;
  border-radius: 15px;
  border: 1px solid #eaeaea;
}

.hotel__heading {
  display: flex;
  justify-content: space-between;
}

.hotel__info {
  display: flex;
  margin-top: -0.5rem;
}

.hotel__info > * {
  margin-right: 0.5rem;
  margin-top: 0.5rem;
}

.hotel__body {
  margin-top: 1rem;
}

.rating {
  display: flex;
}
</style>
