<template>
  <div>
    <div v-for="(item, index) in items" :key="index">
      <h2>{{ item.title }}</h2>
      <p>{{ item.description }}</p>
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

interface Data {
  items: Contents[]
  slug: string
  params: string[]
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
      slug: params.slug,
      params,
    }
  },
  data(): Data {
    return {
      items: [],
      slug: '',
      params: [],
    }
  },
  head() {
    return {
      // @ts-ignore
      title: this.items[0].title,
    }
  },
})
</script>
