
<script setup lang="ts">
import {useRoute} from 'vue-router'
import {useConfigStore} from '@/store/config'

//@ts-ignore
const {setTheme, theme, setSize, size, setLanguage} = useConfigStore()
const route = useRoute()

</script>

  <template>

    <el-card>
      <template #header>
        <div class="card-header">
          <span>看板</span>
          <!-- <el-button class="button" type="text">操作按钮</el-button> -->
        </div>
      </template>

      <el-row :gutter="12">
        <el-col :span="6">
          <el-card class="box-card">
            <template #header>
              <div class="div-header">
                <span>obj模型</span>
              </div>
            </template>
            <div v-for="o in 4" :key="o" class="text item">{{'列表内容 ' + o }}</div>
          </el-card>
        </el-col>

        <el-col :span="6">
          <el-card class="box-card">
            <template #header>
              <div class="div-header">
                <span>stl模型</span>
              </div>
            </template>
            <div v-for="o in 4" :key="o" class="text item">{{'列表内容 ' + o }}</div>
          </el-card>
        </el-col>

        <el-col :span="6">
            <el-card class="box-card">
              <template #header>
                <div class="div-header">
                  <span>ply模型</span>
                </div>
              </template>
              <div v-for="o in 4" :key="o" class="text item">{{'列表内容 ' + o }}</div>
            </el-card>
        </el-col>

        <el-col :span="6">
            <el-card class="box-card">
              <template #header>
                <div class="div-header">
                  <span>卡片名称</span>
                </div>
              </template>
              <div v-for="o in 4" :key="o" class="text item">{{'列表内容 ' + o }}</div>
            </el-card>
        </el-col>

        <el-col :span="6">
            <el-card class="box-card">
              <template #header>
                <div class="div-header">
                  <span>卡片名称</span>
                </div>
              </template>
              <div v-for="o in 4" :key="o" class="text item">{{'列表内容 ' + o }}</div>
            </el-card>
        </el-col>

        <el-col :span="6">
            <el-card class="box-card">
              <template #header>
                <div class="div-header">
                  <span>卡片名称</span>
                </div>
              </template>
              <div v-for="o in 4" :key="o" class="text item">{{'列表内容 ' + o }}</div>
            </el-card>
        </el-col>

        <el-col :span="6">
            <el-card class="box-card">
              <template #header>
                <div class="div-header">
                  <span>卡片名称</span>
                </div>
              </template>
              <div v-for="o in 4" :key="o" class="text item">{{'列表内容 ' + o }}</div>
            </el-card>
        </el-col>

        <el-col :span="6">
            <el-card class="box-card">
              <template #header>
                <div class="div-header">
                  <span>卡片名称</span>
                </div>
              </template>
              <div v-for="o in 4" :key="o" class="text item">{{'列表内容 ' + o }}</div>
            </el-card>
        </el-col>
        
      </el-row>
    </el-card>
    
</template>


<style>
  .card-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-weight: bold;
  }

  .card-main{
    /* display: flex; */
    flex-direction: row;
  }

  .div-header {
    display: flex;
    justify-content:center;
    align-items: center;
  }

  .text {
    font-size: 14px;
  }

  .item {
    margin-bottom: 18px;
  }

  .box-card {
    width: 380px;
    margin-bottom: 15px;
    margin-top: 15px;

    margin-left: 13px;
    margin-right: 15px;
  }
</style>
