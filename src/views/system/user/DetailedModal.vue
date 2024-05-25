<script setup>
import { ElMessage } from 'element-plus'
import {addUser, getUser, getUserInfo, updateUser} from '@/api/user'
import { useDict } from '@/hooks/use-data-dict'
import { resetData } from '@/hooks/use-common'
import  * as  dept from '@/api/dept'
import  * as  post from '@/api/post'
import  * as  role from '@/api/role'

import {handleTree} from "@/views/system/menu/index-hook";

//element valid
const formRules = useElement().formRules
/** 提交按钮 */
const userRef = ref('')
const open = ref(false)
const title = ref('新增用户')
const emits = defineEmits([])
// eslint-disable-next-line camelcase
const { sys_user_sex, sys_normal_disable } = useDict(['sys_user_sex', 'sys_normal_disable'])
const addEditForm = reactive({
  nickName: '',
  userId: null,
  deptId: '',
  phonenumber: '',
  email: '',
  userName: '',
  password: '',
  sex: '',
  status: '',
  postIds: [],
  roleIds: [],
  remark: ''
})
//userRoleList
//userPostList
const formString = JSON.stringify(addEditForm)

const submitForm = () => {
  userRef.value.validate((valid) => {
    if (valid) {
       addEditForm.userRoleList=roleIdsOptions.value.filter((f)=>addEditForm.roleIds.includes(f.roleId)).map(m=>{
         return {userId:addEditForm.userId,roleId:m.roleId}
       })
       addEditForm.userPostList=postIdsOptions.value.filter((f)=>addEditForm.postIds.includes(f.postId)).map(m=>{
         return {userId:addEditForm.userId,postId:m.postId}
       })
      if (addEditForm.userId) {
        updateUser(addEditForm).then(() => {
          ElMessage({ message: '修改成功', type: 'success' })
          open.value = false
          emits('getList')
        })
      } else {
        addUser(addEditForm).then(() => {
          ElMessage({ message: '新增成功', type: 'success' })
          open.value = false
          emits('getList')
        })
      }
    }
  })
}

/** 取消按钮 */
const cancel = () => {
  open.value = false
  resetData(addEditForm, formString)
}

const postIdsOptions = ref([])
const roleIdsOptions = ref([])

const showModal = (row) => {
  
  if (row.userId) {
    getUser(row.userId).then(({ data }) => {
      console.log("详情信息", data)
      //edit
      reshowData(addEditForm, data)
      //edit modal
      title.value = '用户详情'
    })
  } else {
    addEditForm.password = ''
    title.value = '新增用户'
  }
  open.value = true
  //请求岗位和角色
  post.listReq().then(({data})=>{
    postIdsOptions.value = data
  })
  role.listReq().then(({data})=>{
    roleIdsOptions.value = data
  })
}

const reshowData = (addEditForm, detailData) => {
  Object.keys(addEditForm).forEach((fItem) => {
    console.log()
    if (detailData[fItem]) {
      addEditForm[fItem] = detailData[fItem]
    }
  })
}
const deptIdOptions = ref([])
const getDeptData = () => {
  dept.listReq({}).then(({ data }) => {
    deptIdOptions.value = handleTree(data, 'deptId')
  })
}
onMounted(() => {
  getDeptData()
})
//导出给refs使用
defineExpose({ showModal })
</script>

<template>
  <!-- 添加或修改用户配置对话框 -->
  <el-dialog
    v-model="open"
    :close-on-press-escape="false"
    :close-on-click-modal="false"
    :destroy-on-close="true"
    :title="title"
    width="650px"
    append-to-body
  >
    <el-form ref="userRef" :model="addEditForm" label-width="100px" inline disabled>
      <el-form-item label="用户昵称" prop="nickName">
        <el-input v-model="addEditForm.nickName" readonly style="width: 150px" placeholder="用户昵称" />

      </el-form-item>
      <el-form-item label="归属部门" prop="deptId" readonly>
        <!-- <el-input v-model="addEditForm.deptId" readonly style="width: 150px" /> -->
        <el-tree-select
            v-model="addEditForm.deptId"
            style="width: 150px !important"
            filterable
            default-expand-all
            :data="deptIdOptions"
            :props="{ value: 'deptId', label: 'deptName', children: 'children' }"
            value-key="id"
            placeholder="请选择归属部门"
            check-strictly
            disabled="false"
        />
      </el-form-item>
      <el-form-item label="手机号码" prop="phonenumber" >
        <el-input v-model="addEditForm.phonenumber" readonly style="width: 150px" placeholder="手机号码" />
      </el-form-item>
      <el-form-item label="邮箱" prop="email">
        <el-input v-model="addEditForm.email" readonly style="width: 150px" placeholder="邮箱" />
      </el-form-item>
      <el-form-item
        v-if="!addEditForm.userId"
        label="用户名称"
        prop="userName"
        readonly
      >
        <el-input v-model="addEditForm.userName" style="width: 150px" placeholder="用户名称" />
      </el-form-item>
      <el-form-item
        v-if="!addEditForm.userId"
        label="用户密码"
        prop="password"
        readonly
      >
        <el-input v-model="addEditForm.password" style="width: 150px" placeholder="用户密码" />
      </el-form-item>
      <el-form-item label="用户性别" readonly>
        <!-- <el-input v-model="addEditForm.sex" readonly style="width: 150px" /> -->
        <el-select v-model="addEditForm.sex" disabled="false" placeholder="用户性别" style="width: 150px">
          <el-option
            v-for="dict in sys_user_sex"
            :key="dict.value"
            style="width: 150px"
            :label="dict.label"
            :value="dict.value"
            
          />
        </el-select>
      </el-form-item>
      <el-form-item  label="岗位" :rules="formRules.notValid('请选择岗位')">
        <el-select v-model="addEditForm.postIds" disabled="false" multiple placeholder="岗位" style="width: 400px">
          <el-option
              v-for="item in postIdsOptions"
              :key="item.postId"
              :disabled="item.status == 1"
              :label="item.postName"
              :value="item.postId"
             
          />
        </el-select>
      </el-form-item>
      <el-form-item label="角色" :rules="formRules.notValid('请选择角色')">
        <el-select v-model="addEditForm.roleIds"  disabled="false" multiple placeholder="角色" style="width: 400px">
          <el-option
            v-for="item in roleIdsOptions"
            :key="item.roleId"
            :disabled="item.status == 1"
            :label="item.roleName"
            :value="item.roleId"
           
          />
        </el-select>
      </el-form-item>
      <div>
        <el-form-item label="备注" prop="remark">
          <el-input
            v-model="addEditForm.remark"
            type="textarea"
            maxlength="200"
            show-word-limit="show-word-limit"
            resize="none"
            style="width: 300px"
            rows="2"
            clearable="clearable"
            placeholder="请输入备注"
            disabled="false"
          />
        </el-form-item>
      </div>
    </el-form>
    
  </el-dialog>
</template>


<style scoped lang="scss"></style>
