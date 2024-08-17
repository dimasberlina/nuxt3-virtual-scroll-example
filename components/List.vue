<template>
  <div
    v-for="{ data } in items"
    :key="data.id"
    class="item"
    :style="`aspect-ratio: ${data.width / data.height};`"
  >
    <img
      :src="data.src.large"
      :alt="data.alt"
    />
  </div>
</template>

<script setup lang="ts">
import type { UseVirtualListItem } from '@vueuse/core'
import type { Photo } from '../types/Photo'

interface Props {
  items: UseVirtualListItem<Photo>[]
}

defineProps<Props>()
</script>

<style scoped>
.item {
  border-radius: 2%;
  background: rgb(127, 127, 127, 0.2);
  width: 100%;
  filter: drop-shadow(1rem 1rem 1.5rem black);
  animation: fadeIn 2s;
  overflow: hidden;

  img {
    width: 100%;
  }
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>