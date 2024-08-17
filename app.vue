<template>
  <main>
    <h1 class="header">- virtual scroll -</h1>
    <div v-bind="containerProps" class="container">
      <div v-bind="wrapperProps" class="wrapper">
        <List :items="list" />
      </div>
    </div>
  </main>
</template>

<script setup lang="ts">
import { useVirtualList, useInfiniteScroll } from '@vueuse/core'
import type { ImageSearchResponse, Photo } from './types/Photo'

const BASE_API_URL = 'https://image-search.deno.dev'

const page = ref(1)
const items = ref<Photo[]>([])

const fetchData = async () => {
  try {
    const response = await $fetch<ImageSearchResponse>(`${BASE_API_URL}/?page=${page.value}&q=nature`)
    page.value = response.page + 1
    response.photos.length && items.value.push(...response.photos)
  } catch (error) {
    console.error('Error fetching data:', error)
  }
}

await fetchData()

const { list, containerProps, wrapperProps } = useVirtualList(
  items,
  {
    // 400 is a value from max-width from wrapper class
    itemHeight: i => ((items.value[i].height / items.value[i].width) * 400),
    overscan: 10,
  },
)

useInfiniteScroll(
  containerProps.ref,
  async () => { await fetchData() },
  { distance: 500 }
)
</script>

<style>
html {
  background: lightblue;
  overflow: hidden;
}
body {
  margin: 0;
}
</style>

<style scoped>
.header {
  text-align: center;
  letter-spacing: 16px;
}
.container {
  height: 100vh;
}
.wrapper {
  display: flex;
  flex-direction: column;
  place-items: center;
  max-width: 400px;
  width: 100%;
  margin: 0 auto;
  gap: 1rem;
}
</style>