<template>
  <div
    :class="
      twMerge(
        'grid gap-2',
        truncateProxyName && 'max-h-96 overflow-y-auto',
        twoColumnNodeForMobile ? 'grid-cols-2' : 'grid-cols-1',
        isSiderbarCollapsed
          ? hasTwoColumns
            ? 'sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 xl:grid-cols-2 2xl:grid-cols-3'
            : 'sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 xl:grid-cols-5 2xl:grid-cols-6 3xl:grid-cols-7'
          : hasTwoColumns
            ? 'sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-2 3xl:grid-cols-3'
            : 'sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 2xl:grid-cols-5 3xl:grid-cols-6',
      )
    "
  >
    <slot />
  </div>
</template>

<script lang="ts" setup>
import { useProxies } from '@/composables/proxies'
import { isSiderbarCollapsed, truncateProxyName, twoColumnNodeForMobile } from '@/store/settings'
import { twMerge } from 'tailwind-merge'

const { hasTwoColumns } = useProxies()
</script>

<style scoped>
.grid {
  scrollbar-width: thin;
}
</style>
