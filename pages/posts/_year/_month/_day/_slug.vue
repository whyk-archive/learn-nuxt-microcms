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
export default Vue.extend({
  async asyncData({ params, $axios }) {
    const { contents } = await $axios.$get(
      `https://whyk-test.microcms.io/api/v1/list?filters=slug[equals]${params.slug}`,
      {
        headers: {
          'X-API-KEY': process.env.CMS_API_KEY,
        },
      }
    )
    return {
      items: contents,
      slug: params.slug,
      params,
    }
  },
  data() {
    return {
      items: [],
      slug: '',
      params: [],
    }
  },
})
</script>
