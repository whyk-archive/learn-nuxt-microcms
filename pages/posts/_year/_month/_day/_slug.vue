<template>
  <div>
    <div>
      <h2>{{ items[0].title }}</h2>
      <p>{{ items[0].description }}</p>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from 'vue'

interface Contents {
  id: string
  createdAt: string
  updatedAt: string
  publishedAt: string
  title: string
  description: string
  slug: string
}

interface Params {
  year?: string
  month?: string
  day?: string
  slug?: string
}

interface Data {
  items: Contents[]
  params: Params
}

export default Vue.extend({
  async asyncData({ params, $axios }) {
    const res = await $axios.$get(
      `https://whyk-test.microcms.io/api/v1/list?filters=slug[equals]${params.slug}`,
      {
        headers: {
          'X-API-KEY': process.env.CMS_API_KEY,
        },
      }
    )
    const contents: Contents[] = res.contents

    return {
      items: contents,
      params,
    }
  },
  data(): Data {
    return {
      items: [],
      params: {},
    }
  },
  head() {
    return {
      // @ts-ignore
      title: this.items[0].title,
      meta: [
        {
          hid: 'description',
          name: 'description',
          // @ts-ignore
          content: this.items[0].description,
        },
        { hid: 'og:type', name: 'og:type', content: 'article' },
        {
          hid: 'twitter:title',
          name: 'twitter:title',
          // @ts-ignore
          content: `${this.items[0].title} - Learn Nuxt and microCMS`,
        },
        {
          hid: 'twitter:description',
          name: 'twitter:description',
          // @ts-ignore
          content: this.items[0].description,
        },
        {
          hid: 'og:title',
          name: 'og:title',
          // @ts-ignore
          content: `${this.items[0].title} - Learn Nuxt and microCMS`,
        },
        {
          hid: 'og:description',
          name: 'og:description',
          // @ts-ignore
          content: this.items[0].description,
        },
        {
          hid: 'og:url',
          name: 'og:url',
          // @ts-ignore
          content: `https://learn-nuxt-microcms.netlify.app/posts/${this.params.year}/${this.params.month}/${this.params.day}/${this.params.slug}`,
        },
        {
          hid: 'og:site_name',
          name: 'og:site_name',
          // @ts-ignore
          content: this.items[0].title,
        },
      ],
    }
  },
})
</script>
