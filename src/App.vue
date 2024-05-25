
<script setup lang="ts">
import { onBeforeMount, onMounted } from 'vue'
//element-plus lang
import zh from 'element-plus/dist/locale/zh-cn.mjs'
import en from 'element-plus/dist/locale/en.mjs'

// import { storeToRefs } from 'pinia/dist/pinia'
import { storeToRefs } from 'pinia'

import { useRoute } from 'vue-router'
import { useBasicStore } from '@/store/basic'
import { useConfigStore } from '@/store/config'
// import { useErrorLog } from '@/hooks/use-error-log'

//reshow default setting
import { toggleHtmlClass } from '@/theme/utils'
const lang = { zh, en }

const { settings } = storeToRefs(useBasicStore())
const { size, language } = storeToRefs(useConfigStore())


onBeforeMount(() => {
  //set tmp token when setting isNeedLogin false
  
  console.log("111111111", settings.value.isNeedLogin)
  //@ts-ignore
  if (!settings.value.isNeedLogin) useBasicStore().setToken(settings.value.tmpToken)
    
})

onMounted(() => {
  //lanch the errorLog collection
  // useErrorLog()
})
const route = useRoute()
onMounted(() => {
  //@ts-ignore
  const { setTheme, theme, setSize, size, setLanguage, language } = useConfigStore()
  setTheme(theme)
  setLanguage(language, route.meta?.title)
  setSize(size)
  toggleHtmlClass(theme)
})
</script>

<template>
  <el-config-provider :locale="lang[language]" namespace="el" :size="size">
    <router-view />
  </el-config-provider>
</template>

<style lang="scss">
//修改进度条样式
#nprogress .bar {
  background: var(--pregress-bar-color) !important;
}
</style>
