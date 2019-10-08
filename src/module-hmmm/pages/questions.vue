<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card>
        <!-- 新增，导入行 -->
        <el-row style="margin-bottom:10px">
          <el-button type="primary">新增试题</el-button>
          <el-button type="primary">批量导入</el-button>
        </el-row>
        <!--第一个行 学科  -->
        <el-row :gutter="5" style="margin-bottom:10px">
          <el-col :span="4">
            学科：
            <el-select v-model="searchForm.subjectID" clearable>
              <el-option
                v-for="item in subjectIDList"
                :key="item.value"
                :value="item.value"
                :label="item.label"
              ></el-option>
            </el-select>
          </el-col>
          <el-col :span="4">
            难度：
            <el-select v-model="searchForm.difficulty" clearable>
              <el-option
                v-for="item in difficultyList"
                :key="item.value"
                :value="item.value"
                :label="item.label"
              ></el-option>
            </el-select>
          </el-col>
          <el-col :span="4">
            试题类型：
            <el-select v-model="searchForm.questionType" style="width:180px" clearable>
              <el-option
                v-for="item in questionTypeList"
                :key="item.value"
                :value="item.value"
                :label="item.label"
              ></el-option>
            </el-select>
          </el-col>
          <el-col :span="4">
            标签:
            <el-select placeholder="请选择" v-model="searchForm.tags" clearable>
              <el-option
                v-for="item in tagsList"
                :key="item.value"
                :value="item.value"
                :label="item.label"
              ></el-option>
            </el-select>
          </el-col>
          <el-col :span="4">
            关键字：
            <el-input
              type="text"
              placeholder="请输入关键字"
              v-model="searchForm.keyword"
              style="width:190px"
            ></el-input>
          </el-col>
          <el-col :span="4">
            二级目录：
            <el-select
              placeholder="请选择"
              v-model="searchForm.catalogID"
              style="width:135px"
              clearable
            >
              <el-option
                v-for="item in catalogIDList"
                :key="item.id"
                :value="item"
                :label="item.label"
              ></el-option>
            </el-select>
          </el-col>
        </el-row>
        <!-- 第二行 城市 -->
        <el-row style="margin-bottom:10px" :gutter="5">
          <el-col :span="5">
            城市：
            <el-select
              @change="getCitys"
              placeholder="城市"
              v-model="searchForm.province"
              style="width:140px"
            >
              <el-option v-for="item in provinces()" :key="item" :value="item" :label="item"></el-option>
            </el-select>
            <el-select placeholder="区县" v-model="searchForm.city" style="width:140px">
              <el-option v-for="item in cityList" :key="item" :label="item" :value="item"></el-option>
            </el-select>
          </el-col>
          <el-col :span="4">
            题目备注：
            <el-input
              type="text"
              placeholder="请输入备注"
              v-model="searchForm.remarks"
              style="width:180px"
            ></el-input>
          </el-col>
          <el-col :span="3">
            录入人：
            <el-select
              placeholder="请选择"
              v-model="searchForm.creatorID"
              style="width:130px"
              clearable
            >
              <el-option
                v-for="item in creatorIDList"
                :key="item.id"
                :value="item.id"
                :label="item.username"
              ></el-option>
            </el-select>
          </el-col>
          <el-col :span="4">
            企业简称：
            <el-input
              type="text"
              placeholder="请输入简称"
              v-model="searchForm.shortName"
              style="width:180px"
            ></el-input>
          </el-col>
          <el-col :span="3">
            方向：
            <el-select
              placeholder="请选择"
              v-model="searchForm.direction"
              style="width:135px"
              clearable
            >
              <el-option v-for="item in directionList" :key="item" :value="item" :label="item"></el-option>
            </el-select>
          </el-col>
        </el-row>
        <!-- 清除，搜索 行 -->
        <el-row>
          <el-col :span="6" style="margin-left:1450px">
            <el-button size="mini">清除</el-button>
            <el-button type="primary" size="mini">搜索</el-button>
          </el-col>
        </el-row>
      </el-card>
      <!-- table表格 -->
      <el-card style="margin-top:15px">
        <el-table :data="questionsList" stripe style="width: 100%">
          <el-table-column type="index" label="序号" width="180"></el-table-column>
          <el-table-column prop="number" label="试题编号" width="180"></el-table-column>
          <el-table-column prop="subjectID" label="学科"></el-table-column>
          <el-table-column prop="questionType" label="题型"></el-table-column>
          <el-table-column prop="question" label="题干"></el-table-column>
          <el-table-column label="录入时间"></el-table-column>
          <el-table-column prop="difficulty" label="难度"></el-table-column>
          <el-table-column prop="creator" label="录入人"></el-table-column>
          <el-table-column label="操作"></el-table-column>
        </el-table>
      </el-card>
    </div>
  </div>
</template>

<script>
// 3.把api数据接口相关方法导入进来,导入进来想要调用它，通过方法调用 学科
import { simple } from '@/api/hmmm/subjects'
// 难度的相关方法导入(常量)  as给导入的成员设置别名，这样省略了方法中设置以及钩子函数的设置
// 试题类型的相关方法的导入（常量）
// 方向目录的相关方法导入
import {
  difficulty as difficultyList,
  questionType as questionTypeList,
  direction as directionList
} from '@/api/hmmm/constants'
// 标签页的相关方法导入（常量）
import { simple as tagsList } from '@/api/hmmm/tags'
// 录入人的相关方法导入（常量）
import { simple as creatorIDList } from '@/api/base/users'
// 二级目录的相关方法导入
import { simple as catalogIDList } from '@/api/hmmm/directorys'
// 城市
import { provinces, citys } from '@/api/hmmm/citys'
// 基础题库列表数据
import {list} from '@/api/hmmm/questions'
export default {
  name: 'QuestionsList',
  data() {
    return {
      // 2. 定义各个搜索表单域的数据展示成员
      subjectIDList: [],
      difficultyList, // 简易成员赋值 难度
      questionTypeList, // 试题类型
      tagsList: [], // 标签
      cityList: [],
      directionList, // 方向
      creatorIDList: [], // 录入人
      catalogIDList: [], // 二级目录
      // 1. 定义搜索数据对象
      searchForm: {
        subjectID: '', // 学科
        difficulty: '', // 难度
        questionType: '', // 试题类型
        tags: '', // 标签
        province: '',
        city: '',
        direction: '', // 方向
        creatorID: '', // 录入人
        catalogID: '' // 二级目录
      }
    }
  },
  created() {
    // 5.刷新页面就显示
    this.getSubjectIDList()
    this.getTagsList()
    this.getCreatorIDList()
    this.getCatalogIDList()
    // 基础题库
    this.getQuestionsList()
  },
  methods: {
    // 4.获得学科下拉列表数据  学科
    async getSubjectIDList() {
      var rst = await simple()
      // 6.把获得到的学科信息赋予给subjectIDList成员
      this.subjectIDList = rst.data
    },
    // 标签
    async getTagsList() {
      var res = await tagsList()
      this.tagsList = res.data
    },
    // 录入人
    async getCreatorIDList() {
      var rst = await creatorIDList()
      this.creatorIDList = rst.data
    },
    // 二级目录
    async getCatalogIDList() {
      var rst = await catalogIDList()
      this.catalogIDList = rst.data
    },
    // 城市
    provinces,
    // 获得城市信息
    // getCitys方法在模板中声明的时候，没有设置()
    // 这个pname形参就代表被选中的省份信息
    getCitys(pname) {
      this.searchForm.city = '' // 清除之前选取好的城市
      this.cityList = citys(pname)
    },
    // 基础题库 获取列表
    async getQuestionsList() {
      var rst = await list(this.searchForm)
      // confirm.log(rst) 
      // 把获取好的题库数据列表，赋予给questionsList
      this.questionsList = rst.data.items
    }
  }
}
</script>

<style scoped>
</style>
