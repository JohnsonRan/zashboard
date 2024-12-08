<template>
  <div class="w-128 drawer md:drawer-open">
    <input
      id="sidebar"
      type="checkbox"
      class="drawer-toggle"
    />

    <SideBar />

    <div
      class="drawer-content fixed bottom-0 flex h-full w-full flex-col overflow-hidden bg-base-200/50 md:relative md:w-auto"
    >
      <component
        v-if="ctrlComp && isSiderbarCollapsed"
        :is="ctrlComp"
        :horizontal="true"
      />
      <RouterView :class="`${isPWA ? 'mb-24' : 'mb-12'} md:mb-0`" />

      <div
        :class="`fixed bottom-0 z-30 w-full bg-base-200 md:hidden ${isPWA ? 'h-24 pb-12' : 'h-12'}`"
      >
        <div class="flex h-12 w-full items-center justify-center gap-1 p-2">
          <ul class="menu menu-horizontal flex flex-1">
            <li
              v-for="r in routes"
              :key="r"
              class="flex-1"
            >
              <a
                class="flex items-center justify-center"
                :class="r === route.name ? 'active' : 'inactive'"
                :href="`#${r}`"
              >
                <component
                  :is="routeIconMap[r]"
                  class="h-5 w-5"
                />
              </a>
            </li>
          </ul>
          <label for="sidebar">
            <div
              class="btn btn-circle drawer-button btn-sm bg-neutral text-neutral-content shadow-lg"
            >
              <Bars3Icon class="h-4 w-4" />
            </div>
          </label>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import ConnectionCtrl from '@/components/sidebar/ConnectionCtrl.vue'
import LogsCtrl from '@/components/sidebar/LogsCtrl.vue'
import ProxiesCtrl from '@/components/sidebar/ProxiesCtrl.vue'
import RulesCtrl from '@/components/sidebar/RulesCtrl.vue'
import SideBar from '@/components/sidebar/SideBar.vue'
import { useProxies } from '@/composables/proxies'
import { rulesTabShow } from '@/composables/rules'
import { PROXY_TAB_TYPE, ROUTE_NAME, RULE_TAB_TYPE } from '@/config'
import { fetchConfigs } from '@/store/config'
import { initConnections } from '@/store/connections'
import { initLogs } from '@/store/logs'
import { fetchProxies } from '@/store/proxies'
import { fetchRules } from '@/store/rules'
import { isSiderbarCollapsed } from '@/store/settings'
import { activeUuid } from '@/store/setup'
import { initSatistic } from '@/store/statistics'
import {
  ArrowsRightLeftIcon,
  Bars3Icon,
  Cog6ToothIcon,
  DocumentTextIcon,
  GlobeAltIcon,
  WrenchScrewdriverIcon,
} from '@heroicons/vue/24/outline'
import { computed, watch } from 'vue'
import { RouterView, useRoute } from 'vue-router'

const isPWA = (() => {
  return window.matchMedia('(display-mode: standalone)').matches || navigator.standalone
})()

const routeIconMap = {
  [ROUTE_NAME.proxies]: GlobeAltIcon,
  [ROUTE_NAME.connections]: ArrowsRightLeftIcon,
  [ROUTE_NAME.rules]: WrenchScrewdriverIcon,
  [ROUTE_NAME.logs]: DocumentTextIcon,
  [ROUTE_NAME.settings]: Cog6ToothIcon,
}

const ctrlsMap = {
  [ROUTE_NAME.connections]: ConnectionCtrl,
  [ROUTE_NAME.logs]: LogsCtrl,
  [ROUTE_NAME.proxies]: ProxiesCtrl,
  [ROUTE_NAME.rules]: RulesCtrl,
}

const route = useRoute()
const routes = Object.values(ROUTE_NAME)
const ctrlComp = computed(() => {
  return ctrlsMap[route.name as keyof typeof ctrlsMap]
})

const { proxiesTabShow } = useProxies()

watch(
  activeUuid,
  () => {
    rulesTabShow.value = RULE_TAB_TYPE.RULES
    proxiesTabShow.value = PROXY_TAB_TYPE.PROXIES
    fetchConfigs()
    fetchProxies()
    fetchRules()
    initConnections()
    initLogs()
    initSatistic()
  },
  {
    immediate: true,
  },
)
</script>
