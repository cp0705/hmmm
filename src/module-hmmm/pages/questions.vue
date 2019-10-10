<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card>
        <el-row>
          <el-button type="primary">新增试题</el-button>
          <el-button type="danger">批量导入</el-button>
        </el-row>
        <!-- 第一行 -->
        <el-row :gutter='11' style="margin-top:15px">
          <el-col :span="6">
            <span>学科：</span>
            <el-select v-model="formData.subjectID">
              <!-- 显示是根据label，value是把值保存在这里 -->
              <el-option v-for='item in subjectList' :key='item.value'
              :label='item.label' 
              :value='item.value'></el-option>
            </el-select>
          </el-col>
          <el-col :span="6">
            <span>难度：</span>
            <el-select v-model="formData.difficulty">
              <el-option v-for="item in difficulty" :key='item.value' 
              :value='item.value'
              :label='item.label'></el-option>
            </el-select>
          </el-col>
          <el-col :span="6">
            <span>试题类型：</span>
            <el-select v-model="formData.questionType">
              <el-option v-for='item in questionType' :key='item.value'
              :label='item.label'
              :value='item.value'></el-option>
            </el-select>
          </el-col>
          <el-col :span="6">
            <span>标签：</span>
            <el-select v-model="formData.tags">
              <el-option v-for="item in tagsList" :key='item.value'
              :value='item.value'
              :label='item.label'></el-option>
            </el-select>
          </el-col>
        </el-row>
        <!-- 第二行 -->
        <el-row :gutter='15' style="margin-top:15px">
          <el-col :span="12">
            <el-row>
              <el-col :span="12">
                <span>城市：</span>
                <el-select v-model="formData.province">
                  <el-option v-for="item in provinces()" :key="item" :label="item" :value="item"></el-option>
                </el-select>
              </el-col>
              <el-col :span="12">
                <el-select v-model="formData.city" style="width:280px"></el-select>
              </el-col>
            </el-row>
          </el-col>
          <el-col :span="6">
            <span>关键词：</span>
            <el-input style="width:210px" v-model="formData.keyword"></el-input>
          </el-col>
          <el-col :span="6">
            <span>题目备注：</span>
            <el-input style="width:210px" v-model="formData.remarks"></el-input>
          </el-col>
        </el-row>
        <!-- 第三行 -->
        <el-row :gutter='15' style="margin-top:15px">
          <el-col :span="6">
            <span>企业简称：</span>
            <el-input style="width:210px" v-model="formData.shortName"></el-input>
          </el-col>
          <el-col :span="6">
            <span>方向：</span>
            <el-input style="width:210px" v-model="formData.direction"></el-input>
          </el-col>
          <el-col :span="6">
            <span>录入人：</span>
            <el-input style="width:210px" v-model="formData.creatorID"></el-input>
          </el-col>
          <el-col :span="6">
            <span>二级目录：</span>
            <el-input style="width:210px" v-model="formData.catalogID"></el-input>
          </el-col>
        </el-row>
      </el-card>
    </div>
  </div>
</template>

<script>
import { provinces } from '@/api/hmmm/citys' // 城市
import { simple as subjectList } from '@/api/hmmm/subjects' // 获取学科
import { difficulty, questionType } from '@/api/hmmm/constants'
import { simple as tagsList } from '../../api/hmmm/tags'
export default {
  name: 'QuestionsList',
  data() {
    return {
      formData: {
        subjectID: '', // 学科
        difficulty: '', // 难度
        questionType: '', // 试题类型
        tags: '', // 标签名称
        province: '', // 城市
        city: '', // 区县
        keyword: '', // 关键字
        remarks: '', // 题目备注
        shortName: '', // 企业简称
        direction: '', // 方向
        creatorID: '', // 录入人
        catalogID: '' // 目录
      },
      subjectList: [], // 学科列表
      difficulty, // 难度
      questionType, // 试题类型
      tagsList: []
    }
  },
  methods: {
    provinces, // 获取城市
    // 获取学科
    async getSubjectList() {
      let result = await subjectList()
      // debugger
      this.subjectList = result.data
      // debugger
    },
    // 获取标签
    async getTagsList() {
      let result = await tagsList()
      // debugger
      this.tagsList = result.data
    }
  },
  created() {
    this.getSubjectList()
    this.getTagsList()
  }
}
</script>

<style scoped>
</style>
