<script setup >
import { ElMessage } from 'element-plus'
import { addPlatform, getPlatform, updatePlatform } from '@/api/platform'
import { resetData, reshowData } from '@/hooks/use-common'

//element valid
const formRules = useElement().formRules
/** 提交按钮 */
const platformRef = ref('')
const open = ref(false)
const title = ref('上传模型')
const emits = defineEmits([])
const uploadType = ref(0)

const addEditForm = reactive({
  fileName: '',
  fileCode: ''
})

const formString = JSON.stringify(addEditForm)
const submitForm = () => {
  
  platformRef.value.validate((valid) => {
    console.log("111111", valid)
    if (valid) {
      open.value = false
      emits('getList')
      // if (addEditForm.fileCode !== '') {
      //   updatePlatform(addEditForm).then(() => {
      //     ElMessage({ message: '修改成功', type: 'success' })
      //     open.value = false
      //     emits('getList')
      //   })
      // } else {
      //   addPlatform(addEditForm).then(() => {
      //     ElMessage({ message: '新增成功', type: 'success' })
      //     open.value = false
      //     emits('getList')
      //   })
      // }
    }
  })
}
/** 取消按钮 */
const cancel = () => {
  open.value = false
  resetData(addEditForm, formString)
  // emits('getList')
}

const showModal = ({ type }) => {
  title.value = '上传模型文件'
  uploadType.value = type
  open.value = true
}

onMounted(() => {})
//导出给refs使用
defineExpose({ cancel, showModal})

</script>

<template>
  <el-dialog
    v-model="open"
    :title="title"
    :close-on-press-escape="false"
    :close-on-click-modal="false"
    :destroy-on-close="true"
    width="500px"
    append-to-body
    @close="cancel"
  >
    <el-form ref="platformRef" :model="addEditForm" label-width="80px">

      <!-- <el-form-item label="文件名称" prop="fileName" :rules="formRules.isNotNull('文件名称不能为空')">
        <el-input v-model="addEditForm.fileName" class="wi-150px" placeholder="平台名称" />
      </el-form-item>

      <el-form-item label="文件编码" prop="fileCode" :rules="formRules.isNotNull('文件编码不能为空')">
        <el-input v-model="addEditForm.fileCode" class="wi-150px" placeholder="平台名称" />
      </el-form-item> -->

      <el-form-item label="" :rules="formRules.isNotNull('文件名称不能为空')">
        <FileUpload ref="refFileUpload" />
      </el-form-item>

    </el-form>

    <template #footer>
      <div class="dialog-footer">
        <el-button type="primary" @click="submitForm">确 定</el-button>
        <el-button type="primary" @click="cancel">取消</el-button>
      </div>
    </template>
  </el-dialog>
</template>


<style scoped lang="scss"></style>
