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
  head() {
    return {
      meta: [
        {
          hid: 'description',
          name: 'description',
          // @ts-ignore
          content: 'これはNuxtJSとmicroCMSを合わせて使えるか確認するページです',
        },
        { hid: 'og:type', name: 'og:type', content: 'website' },
        {
          hid: 'twitter:title',
          name: 'twitter:title',
          // @ts-ignore
          content: 'Learn Nuxt and microCMS',
        },
        {
          hid: 'twitter:description',
          name: 'twitter:description',
          // @ts-ignore
          content: 'これはNuxtJSとmicroCMSを合わせて使えるか確認するページです',
        },
        {
          hid: 'og:title',
          name: 'og:title',
          // @ts-ignore
          content: 'Learn Nuxt and microCMS',
        },
        {
          hid: 'og:description',
          name: 'og:description',
          // @ts-ignore
          content: 'これはNuxtJSとmicroCMSを合わせて使えるか確認するページです',
        },
        {
          hid: 'og:url',
          name: 'og:url',
          // @ts-ignore
          content: 'https://learn-nuxt-microcms.netlify.app',
        },
        {
          hid: 'og:site_name',
          name: 'og:site_name',
          // @ts-ignore
          content: 'Learn Nuxt and microCMS',
        },
      ],
    }
  },
})
</script>

<style>
.container {
  max-width: 1000px;
  width: 100%;
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
}
</style>
