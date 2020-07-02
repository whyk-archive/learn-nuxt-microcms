<template>
  <div class="container">
    <dl v-for="(item, index) in items" :key="index">
      <dt>{{ item.title }}</dt>
      <dd>{{ item.description }}</dd>
      <dd>
        <nuxt-link
          :to="`posts/${getDatetime(item.createdAt, 'year')}/${getDatetime(
            item.createdAt,
            'month'
          )}/${getDatetime(item.createdAt, 'date')}/${item.slug}`"
        >
          posts/{{ item.slug }}へ移動
        </nuxt-link>
      </dd>
    </dl>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  async asyncData({ $axios }) {
    const { contents } = await $axios.$get(
      'https://whyk-test.microcms.io/api/v1/list',
      {
        headers: {
          'X-API-KEY': process.env.CMS_API_KEY,
        },
      }
    )
    return {
      items: contents,
    }
  },
  data() {
    return {
      items: [],
    }
  },
  methods: {
    getDatetime(datetime: string, unit: 'year' | 'month' | 'date') {
      const { year, month, date } = this.getDateUnit(datetime)

      if (unit === 'year') return year
      else if (unit === 'month') return this.zeroPadding(month)
      else return this.zeroPadding(date)
    },
    getDateUnit(date?: string) {
      const getDate = date ? new Date(date) : new Date()
      const daysList = ['日', '月', '火', '水', '木', '金', '土']

      return {
        year: getDate.getFullYear(),
        month: getDate.getMonth(),
        date: getDate.getDate(),
        days: daysList[getDate.getDay()],
        hour: getDate.getHours(),
        minute: getDate.getMinutes(),
        second: getDate.getSeconds(),
      }
    },
    zeroPadding(num: number) {
      return String(num).padStart(2, '0')
    },
  },
})
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
