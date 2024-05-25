<template>
  <div class="imgUpContainer rowSS" style="flex-wrap: wrap">
    <!--图片列表-->
    <div class="imgShowList rowSS" style="flex-wrap: wrap">
      <div v-for="(item, index) in imageList" :key="index" class="imgItem">
        <!--右上角删除按钮-->
        <el-icon class="rightDel" @click="deleteImage(index)">
          <CloseBold />
        </el-icon>
        <img v-if="validIsImage(item.url)" class="imgStyle" :src="item.url" />
        <svg-icon v-else icon-class="job" class="imgStyle" />
        <div class="imageName">{{ item.name }}</div>
      </div>
    </div>
    <!--图片上传-->
    <div class="packingImageContainer columnCC" style="cursor: pointer">
      
      <el-icon class="packingIconStyle">
        <Plus />
      </el-icon>

      <input
        ref="refSettingFile"
        type="file"
        class="inputStyle"
        accept=".stl"
        multiple
        @change="fileOnChange"
      />
    </div>
    
    <!-- <div>
      <el-upload
        ref="refSettingFile"
        type="file"
        class="inputStyle"
        drag
        accept=".stl"
        multiple
        @change="fileOnChange"
        
        >
        <i class="el-icon-upload"></i>
        <div class="el-upload__text">将文件拖到此处，或<em>点击上传</em></div>

        <div style="color:firebrick "><p>只能上传 stl/obj/ply 文件</p></div>
      </el-upload>
    </div> -->

  </div>

</template>

<script setup>
import { reactive, ref } from 'vue'
import { CloseBold, Plus } from '@element-plus/icons-vue'
import { ElMessage } from 'element-plus'
import axiosReq from '@/utils/axios-req'
// import { watermark } from './imgDillUtils'
import {spliceMinioUrl} from "@/hooks/use-common";

const imageList = ref([])

//检验是否是图片
const validIsImage = (url) => {
  return ['.stl', '.obj', '.ply'].some((type) => url.includes(type))
}
const fileOnChange = () => {
  console.log("1111111111")
  const length = imageList.value.length
  if (length >= 5) {
    ElMessage({ message: '文件上传数不能大于5个', type: 'warning' })
    return
  }
  const fileObj = refSettingFile.value.files
  const fileArr = Object.values(fileObj)
  if (fileArr.length) {
    fileArr.forEach((fItem) => {
      const formData = new FormData()
      formData.append('file', fItem)
      fileUploadSave(formData, fItem.name)
    })
  }
}

const refSettingFile = ref(null)
const fileUploadSave = (formData) => {
  axiosReq({
    url: '/system/oss/upload',
    data: formData,
    method: 'post'
  }).then(({ data }) => {
    // filename
    imageList.value.push({
      url:spliceMinioUrl(data.url),
      name: data.fileName
    })
  })
  refSettingFile.value.value = ''
}

const deleteImage = (index) => {
  imageList.value.splice(index, 1)
}
</script>

<style scoped lang="scss">
$packingWidth: 100px;
$borderRadius: 6px;
.imgUpContainer {
}

.imgShowList {
  .imgItem {
    margin-right: 6px;
    position: relative;

    .imgStyle {
      border-radius: $borderRadius;
      width: $packingWidth;
      height: $packingWidth;
    }

    .imageName {
      width: $packingWidth;
      overflow: hidden;
      text-overflow: ellipsis;
      white-space: nowrap;
    }

    .rightDel {
      background-color: #eee;
      border-radius: 14px;
      position: absolute;
      font-size: 16px;
      top: 1px;
      right: 1px;
      cursor: pointer;
    }

    .rightDel:hover {
      background-color: #999;
    }
  }
}

//图片上传
.packingImageContainer {
  position: relative;
  background-color: #fbfdff;
  border: 1px dashed #c0ccda;
  border-radius: $borderRadius;
  box-sizing: border-box;
  width: $packingWidth;
  height: $packingWidth;
  cursor: pointer;

  .packingIconStyle {
    font-size: 30px;
  }

  .inputStyle {
    position: absolute;
    top: 0;
    left: 0;
    opacity: 0;
    width: $packingWidth;
    height: $packingWidth;
  }
}
</style>
