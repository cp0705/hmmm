<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card>
        <el-form :model="seachFrom">
          <el-form-item>
            <el-button type="primary">新增试题</el-button>
            <el-button type="primary">批量导入</el-button>
          </el-form-item>
          <el-form-item>
            <el-col :span="4">
              学科：
              <el-select v-model="seachFrom.subjectID">
                <el-option v-for="item in subjectIDList" :key="item.value" :value="item.value" :label="item.label"></el-option>
              </el-select>
            </el-col>
            <el-col :span="4">
              难度：
              <el-select v-model="seachFrom.difficulty"></el-select>
            </el-col>
            <el-col :span="4">
              试题类型：
              <el-select v-model="seachFrom.questionType" style="width:180px"></el-select>
            </el-col>
            <el-col :span="4">
              标签：
              <el-select v-model="seachFrom.tags"></el-select>
            </el-col>
            <el-col :span="4">
              关键字：
              <el-select v-model="seachFrom.keyword" style="width:200px"></el-select>
            </el-col>
          </el-form-item>
          <el-form-item>
            <el-col :span="3">
              城市：
              <el-select v-model="seachFrom.province" style="width:150px"></el-select>
            </el-col>
            <el-col :span="3">
              <el-select v-model="seachFrom.city" style="width:178px"></el-select>
            </el-col>
            <el-col :span="4">
              题目备注：
              <el-select v-model="seachFrom.remarks"  style="width:180px"></el-select>
            </el-col>
            <el-col :span="4">
              企业简称：
              <el-select v-model="seachFrom.shortName" style="width:180px"></el-select>
            </el-col>
            <el-col :span="4">
              方向：
              <el-select v-model="seachFrom.direction"></el-select>
            </el-col>
            <el-col :span="4">
              录入人：
              <el-select v-model="seachFrom.creatorID" style="width:140px"></el-select>
            </el-col>
          </el-form-item>
          <el-form-item label="二级目录 :">
            <el-input placeholder="请输入二级目录" style="width:170px"></el-input>
            <el-button style="margin-left:1040px">清除</el-button>
            <el-button type="primary">搜索</el-button>
          </el-form-item>
        </el-form>
      </el-card>
    </div>
  </div>
</template>

<script>
// 3.把api数据接口相关方法导入进来,导入进来想要调用它，通过方法调用
import {simple} from '@/api/hmmm/subjects'
export default {
  name: 'QuestionsList',
  data() {
    return {
      // 2. 定义各个搜索表单域的数据展示成员
      subjectIDList: [],
      difficultyList: [],
      questionTypeList: [],
      tagsList: [],
      keywordList: [],
      provinceList: [],
      cityList: [],
      remarksList: [],
      shortNameList: [],
      directionList: [],
      creatorIDList: [],
      // 1. 定义搜索数据对象
      seachFrom: {
        subjectID: '',
        difficulty: '',
        questionType: '',
        tags: '',
        keyword: '',
        province: '',
        city: '',
        remarks: '',
        shortName: '',
        direction: '',
        creatorID: ''
      }
    }
  },
  created() {
    // 5.刷新页面就显示
    this.getSubjectIDList()
  },
  methods: {
    // 4.获得学科下拉列表数据
    async getSubjectIDList() {
      var rst = await simple()
      // 6.把获得到的学科信息赋予给subjectIDList成员
      this.subjectIDList = rst.data
    }
  }
}
</script>

<style scoped>
</style>
