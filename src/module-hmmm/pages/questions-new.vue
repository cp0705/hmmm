<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card shadow="never">
        <el-form v-model="searchForm">
          <!-- 学科 -->
          <el-form-item label='学科:' required >
            <el-select style="width:450px" v-model="searchForm.subjectID">
              <el-option v-for="item in subjectIDList" :key="item.value"
              :value="item.value" :label="item.label"></el-option>
            </el-select>
          </el-form-item>
          <!-- 目录 -->
          <el-form-item label='目录:' required >
            <el-select style="width:450px" v-model="searchForm.catalogID">
              <el-option v-for="item in directorysList" :key="item.value"
                    :value="item.value" :label="item.label"></el-option>
            </el-select>
          </el-form-item>
          <!-- 企业 -->
          <el-form-item label='企业:' required >
            <el-select style="width:450px" v-model="searchForm.enterpriseID">
              <el-option v-for="item in enterpriseList" :key="item.id"
              :value="item.id" :label="item.company"></el-option>
            </el-select>
          </el-form-item>
          <!-- 城市 -->
          <el-form-item label='城市:' required >
            <el-select style="width:220px;margin-right:7px" v-model="searchForm.province"  @change="getCitysList">
              <el-option v-for="item in provincesList" :key="item"
              :value="item" :label="item"></el-option>
            </el-select>
            <el-select style="width:220px" v-model="searchForm.city">
              <el-option v-for="item in citysList" :key="item"
                      :value="item" :label="item"></el-option>
            </el-select>
          </el-form-item>
          <!-- 方向 -->
          <el-form-item label='方向:' required >
            <el-select style="width:450px" v-model="searchForm.direction">
              <el-option v-for="item in directionList" :key="item"
                    :value="item" :label="item"></el-option>
            </el-select>
          </el-form-item>
          <!-- 题型 -->
          <el-form-item label='题型:' required >
            <el-radio-group v-model="radio" fill="red" :v-model="searchForm.questionType">
              <el-radio :label="3">单选</el-radio>
              <el-radio :label="6">多选</el-radio>
              <el-radio :label="9">简答</el-radio>
            </el-radio-group>
          </el-form-item>
          <!-- 难度 -->
          <el-form-item label='难度:' required >
            <el-radio-group v-model="radio" fill="red" :v-model="searchForm.difficulty">
              <el-radio :label="3">单选</el-radio>
              <el-radio :label="6">多选</el-radio>
              <el-radio :label="9">简答</el-radio>
            </el-radio-group>
          </el-form-item>
          <!-- 题干 -->
          <el-form-item label='题干:' required>
            <el-row></el-row>
            <textarea name="" id="" cols="100" rows="10" v-model="searchForm.question"></textarea>
          </el-form-item>
          <!-- 选项 (以下选中的选项为正确答案) -->
          <el-form-item label='选项 (以下选中的选项为正确答案)' required v-model="searchForm.options">
            <el-row></el-row>
            <el-radio label="A"></el-radio>
            <el-radio label="B"></el-radio>
            <el-radio label="C"></el-radio>
            <el-radio label="D"></el-radio>
            <el-button>+ 增加选项与答案</el-button>
          </el-form-item>
          <!-- 解析视频 -->
          <el-form-item label='解析视频:'>
            <input type="text" placeholder="请输入视频地址，以http(s)://开头" 
            style="width:250px" v-model="searchForm.videoURL">
          </el-form-item>
          <!-- 答案解析： -->
          <el-form-item label='答案解析:' required>
            <el-row></el-row>
            <textarea name="" id="" cols="100" rows="10" v-model="searchForm.answer"></textarea>
          </el-form-item>
          <!-- 题目备注： -->
          <el-form-item label='题目备注:'>
            <el-row></el-row>
            <textarea name="" id="" cols="100" rows="10" v-model="searchForm.remarks"></textarea>
          </el-form-item>
          <!-- 试题标签 -->
          <el-form-item label='试题标签:' required>
            <el-input style="width:250px" placeholder="请输入" v-model="searchForm.tags"></el-input>
          </el-form-item>
        </el-form>
        <!-- 提交按钮 -->
        <el-row style="text-align:center">
          <el-button class="button" type="primary">提交</el-button>
        <el-button class="button">取消</el-button>
        </el-row>
      </el-card>
    </div>
  </div>
</template>

<script>
// 接口引入
import {simple} from '@/api/hmmm/subjects' // 学科
import {simple as directorys} from '@/api/hmmm/directorys'// 目录
import {provinces, citys} from '@/api/hmmm/citys'// 城市
import {direction as directionList} from '@/api/hmmm/constants'// 方向
import {list} from '@/api/hmmm/companys'// 企业
export default {
  name: 'QuestionsNew',
  data() {
    return {
      radio: 3, // 默认题型选项
      subjectIDList: [], // 学科列表
      provincesList: [], // 省份
      citysList: [], // 城区
      directionList, // 方向
      enterpriseList: [], // 企业
      directorysList: [], // 目录
        // 搜索表单
      searchForm: {
        subjectID: '', // 学科 
        catalogID: '', // 目录
        enterpriseID: '', // 企业
        city: '', // 企业所在城市
        province: '', // 企业所在城市
        direction: '', // 方向
        questionType: '', // 试题类型
        difficulty: '', // 难度
        question: '', // 题干
        options: '', // 选项
        videoURL: '', // 解析视频
        answer: '', // 答案解析
        remarks: '', // 题目备注
        tags: ''// 试题标签
      }
    }
  },
  methods: {
    // 获取学科列表
    async getSubjectIDList() {
      var res = await simple()
      this.subjectIDList = res.data
    },
    // 二级目录
    async getdirectorysList() {
      var res = await directorys()
      this.directorysList = res.data
    },
    // 省份/城市
    async getProvincesList() {
      var res = await provinces()
      this.provincesList = res
    },
    // 城区
    async getCitysList(pname) {
      this.searchForm.city = '' // 清除之前选取好的城市
      var res = await citys(pname)
      this.citysList = res
    },
    // 企业
    async getenterpriseList() {
      var res = await list()
      this.enterpriseList = res.data.items
    }
  },
  created() {
     // 学科列表函数调用
    this.getSubjectIDList()
    // 二级目录
    this.getdirectorysList()
    // 省份
    this.getProvincesList()
    // 城区
    this.getCitysList()
    // 企业
    this.getenterpriseList()
  }
}
</script>

<style scoped>
</style>
