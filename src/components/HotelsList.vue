<template>
  <div v-if="listData.length > 0">
    <ul class="grid">
      <HotelsItem
        v-for="(hotel, index) in paginatedData"
        :key="index"
        v-bind:hotel="hotel"
      />
    </ul>
    <div class="pagination">
      <button :disabled="pageNumber === 0" @click="prevPage">
        Назад
      </button>
      <button :disabled="pageNumber >= pageCount - 1" @click="nextPage">
        Вперед
      </button>
    </div>
  </div>

  <h1 v-else>Ничего не найдено</h1>
</template>

<script>
import HotelsItem from './HotelsItem'
export default {
  data() {
    return {
      pageNumber: 0,
    }
  },
  props: {
    listData: {
      type: Array,
      required: true,
    },
    size: {
      type: Number,
      required: false,
      default: 3,
    },
  },
  computed: {
    pageCount() {
      let l = this.listData.length,
        s = this.size
      return Math.ceil(l / s)
    },
    paginatedData() {
      const start = this.pageNumber * this.size,
        end = start + this.size
      return this.listData.slice(start, end)
    },
  },
  methods: {
    nextPage() {
      this.pageNumber++
    },
    prevPage() {
      this.pageNumber--
    },
  },
  components: {
    HotelsItem,
  },
}
</script>

<style>
.pagination {
  margin-top: 2rem;
  text-align: center;
}
.pagination > * {
  margin: 0 1rem;
}
</style>
