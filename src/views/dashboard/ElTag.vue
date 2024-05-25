<script setup lang="ts">
import { getItem } from "@/hooks/use-data-dict";
const props = defineProps({
  // 数据
  dataTypeName: {
    type: String,
    default: null
  },
  // 数据
  options: {
    type: Array,
    default: null
  },
  // 当前的值
  value: [Number, String, Array]
})

//ts中的写法要设置变量的类型
let labelItem: any
labelItem=ref({})

//ts中的写法要设置变量的类型
let getItems: any
getItems = getItem

getItems(props.dataTypeName,props.value).then((res)=>{
  labelItem.value=res
})
const label = computed(() => {
  return labelItem.value.label
})
</script>

<template>
  <el-tag :type="labelItem.elTagType === 'primary' ? '' : labelItem.elTagType">{{label}}</el-tag>
</template>
