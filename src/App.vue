<script setup lang="ts">
import { computed, nextTick, onMounted, ref, watch } from 'vue'
import { RouterView } from 'vue-router'
import { FONTS } from './config'
import { font, theme } from './store/settings'

const app = ref<HTMLElement>()
const setThemeColor = () => {
  const themeColor = getComputedStyle(app.value!).getPropertyValue('background-color').trim()
  const metaThemeColor = document.querySelector('meta[name="theme-color"]')
  if (metaThemeColor) {
    metaThemeColor.setAttribute('content', themeColor)
  }
}

onMounted(() => {
  setThemeColor()
  watch(theme, () => {
    nextTick(setThemeColor)
  })
})

const fontClassMap = {
  [FONTS.MI_SANS]: 'font-MiSans',
  [FONTS.SARASA_UI]: 'font-SarasaUI',
  [FONTS.PING_FANG]: 'font-PingFang',
  [FONTS.FIRA_SANS]: 'font-FiraSans',
  [FONTS.SYSTEM_UI]: 'font-SystemUI',
}
const fontClassName = computed(() => fontClassMap[font.value])
</script>

<template>
  <div
    ref="app"
    :class="`flex h-dvh w-screen overflow-x-hidden bg-base-100 ${fontClassName}`"
    :data-theme="theme"
  >
    <RouterView />
  </div>
</template>
