<template>
  <div class="flex flex-col gap-2 p-2 text-xs">
    <div class="grid grid-cols-2 gap-2">
      <div>{{ $t('connections') }}: {{ activeConnections.length }}</div>
      <div>{{ $t('memoryUsage') }}: {{ prettyBytesHelper(memory, { binary: true }) }}</div>
      <div>{{ $t('download') }}: {{ prettyBytesHelper(downloadTotal) }}</div>
      <div>{{ $t('dlSpeed') }}: {{ prettyBytesHelper(downloadSpeed) }}/s</div>
      <div>{{ $t('upload') }}: {{ prettyBytesHelper(uploadTotal) }}</div>
      <div>{{ $t('ulSpeed') }}: {{ prettyBytesHelper(uploadSpeed) }}/s</div>
    </div>

    <div class="flex gap-1">
      {{ $t('version') }}:
      <img
        :src="isSingBox ? SingBoxLogo : MetacubexLogo"
        class="w-4 rounded-sm"
      />
      {{ version }}
    </div>

    <div class="flex">
      <BackendSwitch class="flex-1" />
      <button
        class="btn btn-circle btn-ghost btn-xs"
        @click="isSiderbarCollapsed = true"
      >
        <ArrowLeftCircleIcon class="h-5 w-5" />
      </button>
    </div>
  </div>
</template>

<script setup lang="ts">
import { isSingBox, version } from '@/api'
import MetacubexLogo from '@/assets/metacubex.jpeg'
import SingBoxLogo from '@/assets/sing-box.svg'
import { prettyBytesHelper } from '@/helper'
import { activeConnections, downloadTotal, uploadTotal } from '@/store/connections'
import { isSiderbarCollapsed } from '@/store/settings'
import { downloadSpeed, memory, uploadSpeed } from '@/store/statistics'
import { ArrowLeftCircleIcon } from '@heroicons/vue/24/outline'
import BackendSwitch from '../settings/BackendSwitch.vue'
</script>
