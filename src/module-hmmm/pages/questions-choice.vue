<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card shadow="never">
        <!-- 按钮 -->
        <el-button class="button">新增试题</el-button>
        <el-button class="button">批量导入</el-button>
        <!-- 搜索栏目 -->
        <el-form v-model="searchForm">
            <el-row>
              <!-- 学科 -->
              <el-col :span='5'>
                <el-form-item label='学科'>
                  <el-select v-model="searchForm.subjectID" style='width:180px' clearable>
                    <el-option v-for="item in subjectIDList" :key="item.value"
                    :value="item.value" :label="item.label"></el-option>
                  </el-select>
                </el-form-item>
              </el-col>
              <!-- 状态 -->
              <el-col :span='5'>
                <el-form-item label='状态'>
                  <el-select v-model="searchForm.chkState" style='width:180px' clearable>
                    <el-option v-for="item in statusList" :key="item.value"
                    :value="item.value" :label="item.label"></el-option>
                  </el-select>
                </el-form-item>
              </el-col>
              <!-- 难度 -->
              <el-col :span='4'>
                <el-form-item label='难度'>
                  <el-select  v-model="searchForm.difficulty" style='width:135px' clearable>
                    <el-option v-for="item in difficultyList" :key="item.value"
                    :value="item.value" :label="item.label"></el-option>
                  </el-select>
                </el-form-item>
              </el-col>
              <!-- 试题类型 -->
              <el-col :span='5'>
                <el-form-item  label='试题类型'>
                  <el-select v-model="searchForm.questionType" style='width:150px' clearable>
                    <el-option v-for="item in questionTypeList" :key="item.value"
                    :value="item.value" :label="item.label"></el-option>
                  </el-select>
                </el-form-item>
              </el-col>
              <!-- 标签 -->
              <el-col :span='5'>
                <el-form-item  label='标签'>
                  <el-select v-model="searchForm.tags" style='width:140px' placeholder="请输入" clearable>
                    <el-option v-for="item in tagsList" :key="item.value"
                    :value="item.value" :label="item.label"></el-option>
                  </el-select>
                </el-form-item>
              </el-col>
              <!-- 录入人 -->
              <el-col :span='4'>
                <el-form-item label='录入人'>
                  <el-select v-model="searchForm.creatorID" style='width:120px' placeholder="请输入" clearable>
                    <el-option v-for="item in userList" :key="item.id"
                    :value="item.id" :label="item.username"></el-option>
                  </el-select>
                </el-form-item>
              </el-col>
              <!-- 关键字 -->
              <el-col :span='6'>
                <el-form-item label='关键字'>
                  <el-input v-model="searchForm.keyword" style='width:210px' placeholder="请输入题目编号/题干"></el-input>
                </el-form-item>
              </el-col>
              <!-- 题目备注 -->
              <el-col :span='4'>
                <el-form-item label='题目备注'>
                  <el-input v-model="searchForm.remarks" style='width:110px' placeholder="请输入"></el-input>
                </el-form-item>
              </el-col>
              <!-- 城市 -->
              <el-col :span='5'>
                <el-form-item label='城市'>
                  <el-select v-model="searchForm.province" style='width:180px' clearable @change="getCitysList">
                    <el-option v-for="item in provincesList" :key="item"
                    :value="item" :label="item"></el-option>
                  </el-select>
                </el-form-item>
              </el-col>
              <el-col :span='5'>
                <el-form-item >
                  <el-select v-model="searchForm.city" style='width:180px' clearable>
                    <el-option v-for="item in citysList" :key="item"
                      :value="item" :label="item"></el-option>
                  </el-select>
                </el-form-item>
              </el-col>
              <!-- 二级目录 -->
              <el-col :span='5'>
                <el-form-item label='二级目录'>
                  <el-select v-model="searchForm.catalogID" style='width:150px' placeholder="请输入二级目录" clearable>
                    <el-option v-for="item in directorysList" :key="item.value"
                    :value="item.value" :label="item.label"></el-option>
                  </el-select>
                </el-form-item>
              </el-col>
              <!-- 企业简称 -->
              <el-col :span='5'>
                <el-form-item v-model="searchForm.shortName" label='企业简称'>
                  <el-input style='width:150px' placeholder="请输入"></el-input>
                </el-form-item>
              </el-col>
              <!-- 方向 -->
              <el-col :span='5'>
                <el-form-item label='方向'>
                  <el-select v-model="searchForm.direction" style='width:140px' placeholder="请输入">
                    <el-option v-for="item in directionList" :key="item"
                    :value="item" :label="item"></el-option>
                  </el-select>
                </el-form-item>
              </el-col>
            </el-row>
            <!-- 标签选择 -->
            <template>
              <el-tabs type="card">
                <el-tab-pane label="全部" ></el-tab-pane>
                <el-tab-pane label="待审核"></el-tab-pane>
                <el-tab-pane label="已审核"></el-tab-pane>
              </el-tabs>
              <!-- 表格数据 -->
              <el-table border :header-cell-style="{
                'background-color': 'rgb(248, 248, 248)','color': '#555','height':'60px',
                'font-weight':'400'
              }">
                <el-table-column label='序号' width='50px' class="table"></el-table-column>
                <el-table-column label='试题编号' width='90px'></el-table-column>
                <el-table-column label='学科' width='80px'></el-table-column>
                <el-table-column label='题型' width='80px'></el-table-column>
                <el-table-column label='题干' width='150px'></el-table-column>
                <el-table-column label='录入时间' width='150px'></el-table-column>
                <el-table-column label='录入人'></el-table-column>
                <el-table-column label='难度' width='60px'></el-table-column>
                <el-table-column label='使用次数'></el-table-column>
                <el-table-column label='审核状态'></el-table-column>
                <el-table-column label='审核意见'></el-table-column>
                <el-table-column label='审核人'></el-table-column>
              </el-table>
            </template>
            <!-- 分页按钮 -->
            <el-pagination type='primary' style="text-align:center" background
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
              :current-page.sync="currentPage3"
              :page-size="100"
              layout="prev, pager, next, jumper"
              :total="1000">
            </el-pagination>
        </el-form>
      </el-card>
    </div>
  </div>
</template>

<script>
// 接口引入
import {simple} from '@/api/hmmm/subjects' // 学科
import {simple as tags} from '@/api/hmmm/tags' // 标签
import {simple as user} from '@/api/base/users' // 录入人
import {simple as directorys} from '@/api/hmmm/directorys'// 目录
import {provinces, citys} from '@/api/hmmm/citys'// 城市
// 常量
import {status as statusList,
        difficulty as difficultyList,
        questionType as questionTypeList,
        direction as directionList
      } from '@/api/hmmm/constants'

export default {
  name: 'QuestionsChoice',
  data() {
    return {
      subjectIDList: [], // 学科列表
      statusList, // 状态
      difficultyList, // 难度
      questionTypeList, // 试题类型
      tagsList: [], // 标签列表
      userList: [], // 录入人
      directionList, // 方向
      directorysList: [], // 二级目录
      provincesList: [], // 省份
      citysList: [], // 城区
      // 搜索表单
      searchForm: {
        subjectID: '', // 学科
        difficulty: '', // 难度
        questionType: '', // 试题类型
        tags: '', // 标签名称
        province: '', // 企业所在城市
        city: '', // 企业所在城市
        keyword: '', // 关键字
        remarks: '', // 题目备注
        shortName: '', // 企业简称
        direction: '', // 方向
        creatorID: '', // 录入人
        catalogID: '', // 目录
        chkState: '' // 审核状态
      }
    }
  },
  methods: {
    // 获取学科列表
    async getSubjectIDList() {
      var res = await simple()
      this.subjectIDList = res.data
    },
    // 获取标签列表
    async getTagsList() {
      var res = await tags()
      this.tagsList = res.data
    },
    // 获取录入人
    async getUserList() {
      var res = await user()
      this.userList = res.data
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
    }
  },
  created() {
    // 学科列表函数调用
    this.getSubjectIDList()
    // 标签
    this.getTagsList()
    // 录入人
    this.getUserList()
    // 二级目录
    this.getdirectorysList()
    // 省份
    this.getProvincesList()
    // 城区
    this.getCitysList()
  }
}
</script>

<style scoped>
.button{
  background-color: #eee;
  color: #333;
  margin-bottom: 10px
}
.table{
  background-color: #ccc;
  height: 100px;
  color:#000
  
}
</style>
