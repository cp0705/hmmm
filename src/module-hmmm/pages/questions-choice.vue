<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card shadow="never">
        <!-- 按钮 -->
        <el-button class="button" @click="questionsAdd">新增试题</el-button>
        <el-button class="button">批量导入</el-button>
        <!-- 搜索栏目 -->
        <el-form v-model="searchForm">
          <el-row>
            <!-- 学科 -->
            <el-col :span="5">
              <el-form-item label="学科">
                <el-select
                  v-model="searchForm.subjectID"
                  style="width:180px"
                  clearable
                  @change="getChoiceList"
                >
                  <el-option
                    v-for="item in subjectIDList"
                    :key="item.value"
                    :value="item.value"
                    :label="item.label"
                  ></el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <!-- 状态 -->
            <el-col :span="5">
              <el-form-item label="状态">
                <el-select
                  v-model="searchForm.status"
                  style="width:180px"
                  clearable
                  @change="getChoiceList"
                >
                  <el-option
                    v-for="item in statusList"
                    :key="item.value"
                    :value="item.value"
                    :label="item.label"
                  ></el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <!-- 难度 -->
            <el-col :span="4">
              <el-form-item label="难度">
                <el-select
                  v-model="searchForm.difficulty"
                  style="width:135px"
                  clearable
                  @change="getChoiceList"
                >
                  <el-option
                    v-for="item in difficultyList"
                    :key="item.value"
                    :value="item.value"
                    :label="item.label"
                  ></el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <!-- 试题类型 -->
            <el-col :span="5">
              <el-form-item label="试题类型">
                <el-select
                  v-model="searchForm.questionType"
                  style="width:150px"
                  clearable
                  @change="getChoiceList"
                >
                  <el-option
                    v-for="item in questionTypeList"
                    :key="item.value"
                    :value="item.value"
                    :label="item.label"
                  ></el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <!-- 标签 -->
            <el-col :span="5">
              <el-form-item label="标签">
                <el-select
                  v-model="searchForm.tags"
                  style="width:140px"
                  placeholder="请输入"
                  clearable
                  @change="getChoiceList"
                >
                  <el-option
                    v-for="item in tagsList"
                    :key="item.value"
                    :value="item.value"
                    :label="item.label"
                  ></el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <!-- 录入人 -->
            <el-col :span="4">
              <el-form-item label="录入人">
                <el-select
                  v-model="searchForm.creatorID"
                  style="width:120px"
                  placeholder="请输入"
                  clearable
                  @change="getChoiceList"
                >
                  <el-option
                    v-for="item in userList"
                    :key="item.id"
                    :value="item.id"
                    :label="item.username"
                  ></el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <!-- 关键字 -->
            <el-col :span="6">
              <el-form-item label="关键字">
                <el-input
                  v-model="searchForm.keyword"
                  style="width:210px"
                  placeholder="请输入题目编号/题干"
                  @change="getChoiceList"
                ></el-input>
              </el-form-item>
            </el-col>
            <!-- 题目备注 -->
            <el-col :span="4">
              <el-form-item label="题目备注">
                <el-input
                  v-model="searchForm.remarks"
                  style="width:110px"
                  placeholder="请输入"
                  @change="getChoiceList"
                ></el-input>
              </el-form-item>
            </el-col>
            <!-- 城市 -->
            <el-col :span="5">
              <el-form-item label="城市">
                <el-select
                  v-model="searchForm.province"
                  style="width:180px"
                  clearable
                  @change="getCitysList"
                >
                  <el-option v-for="item in provincesList" :key="item" :value="item" :label="item"></el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <el-col :span="5">
              <el-form-item>
                <el-select
                  v-model="searchForm.city"
                  style="width:180px"
                  clearable
                  @change="getChoiceList"
                >
                  <el-option v-for="item in citysList" :key="item" :value="item" :label="item"></el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <!-- 二级目录 -->
            <el-col :span="5">
              <el-form-item label="二级目录">
                <el-select
                  v-model="searchForm.catalogID"
                  style="width:150px"
                  placeholder="请输入二级目录"
                  clearable
                  @change="getChoiceList"
                >
                  <el-option
                    v-for="item in directorysList"
                    :key="item.value"
                    :value="item.value"
                    :label="item.label"
                  ></el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <!-- 企业简称 -->
            <el-col :span="5">
              <el-form-item label="企业简称">
                <el-input
                  v-model="searchForm.shortName"
                  style="width:150px"
                  placeholder="请输入"
                  @change="getChoiceList"
                ></el-input>
              </el-form-item>
            </el-col>
            <!-- 方向 -->
            <el-col :span="5">
              <el-form-item label="方向">
                <el-select
                  v-model="searchForm.direction"
                  style="width:140px"
                  placeholder="请输入"
                  @change="getChoiceList"
                  clearable
                >
                  <el-option v-for="item in directionList" :key="item" :value="item" :label="item"></el-option>
                </el-select>
              </el-form-item>
            </el-col>
          </el-row>
          <!-- 标签选择 -->
          <template>
            <el-tabs type="card" v-model="activeName">
              <el-tab-pane label="全部" name="3"></el-tab-pane>
              <el-tab-pane label="待审核" name="0"></el-tab-pane>
              <el-tab-pane label="已审核" name="1"></el-tab-pane>
            </el-tabs>
            <!-- 表格数据 -->
            <el-table
              border
              :data="choiceList"
              :cell-style="cellStyle"
              :header-cell-style="{
                'background-color': 'rgb(248, 248, 248)','color': '#555','height':'60px',
                'font-weight':'400', 'text-align':'center'
              }"
            >
              <el-table-column label="序号" width="50px" type="index"></el-table-column>
              <el-table-column label="试题编号" width="90px" prop="number"></el-table-column>
              <el-table-column label="学科" width="70px" prop="subjectID" :formatter="subjectFMT"></el-table-column>
              <el-table-column
                label="题型"
                width="60px"
                prop="questionType"
                :formatter="questionTypeFMT"
              ></el-table-column>
              <el-table-column label="题干" width="150px" prop="question"></el-table-column>
              <el-table-column label="录入时间" width="160px">
                <span slot-scope="stData">{{stData.row.addDate | parseTimeByString}}</span>
              </el-table-column>
              <el-table-column label="录入人" width="110px" prop="creator"></el-table-column>
              <el-table-column label="难度" width="60px" prop="difficulty" :formatter="difficultyFMT"></el-table-column>
              <el-table-column label="使用次数" width="80px" prop="index"></el-table-column>
              <el-table-column label="审核状态" width="80px" prop="chkState" :formatter="chkStateFMT"></el-table-column>
              <el-table-column label="审核意见" width="80px" prop="chkRemarks"></el-table-column>
              <el-table-column label="审核人" prop="chkUser"></el-table-column>
            </el-table>
          </template>
          <!-- 分页按钮 -->
          <el-pagination
            type="primary"
            style="text-align:center;margin-top:20px"
            background
            :page-count="pageList.total"
            :current-page="pageList.page_current"
            :page-size="pageList.sizes"
            @current-change="changeCurrent"
            layout="prev, pager, next, jumper"
          ></el-pagination>
        </el-form>
      </el-card>
    </div>
  </div>
</template>

<script>
// 接口引入
import { simple } from '@/api/hmmm/subjects' // 学科
import { simple as tags } from '@/api/hmmm/tags' // 标签
import { simple as user } from '@/api/base/users' // 录入人
import { simple as directorys } from '@/api/hmmm/directorys' // 目录
import { provinces, citys } from '@/api/hmmm/citys' // 城市
import { choice } from '@/api/hmmm/questions' // 精选题库列表
// 常量
import {
  status as statusList,
  difficulty as difficultyList,
  questionType as questionTypeList,
  direction as directionList
} from '@/api/hmmm/constants'

export default {
  name: 'QuestionsChoice',
  data() {
    return {
      // 分页
      pageList: {
        total: 0,
        page_current: 1,
        sizes: 10
      },
      // 切换状态栏
      activeName: '3',
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
      choiceList: [], // 精选题库列表
      // 搜索表单
      searchForm: {
        page: '1', // 当前页数
        status: '', // 状态
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
        chkState: '3' // 审核状态
      }
    }
  },
  methods: {
    // 跳转到新增试题页面
    questionsAdd() {
      this.$router.push('new')
    },
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
    },
    // 精选题库列表
    async getChoiceList() {
      var res = await choice(this.searchForm)
      this.choiceList = res.data.items
      this.pageList.total = res.data.pages
      this.pageList.sizes = res.data.pagesize
    },
    // 表格第一列样式
    cellStyle({ row, column, rowIndex, columnIndex }) {
      if (columnIndex === 0) {
        // 指定列号
        return 'color:#108ee9;text-align:center'
      } else {
        return 'text-align:center'
      }
    },
    // 格式化学科
    subjectFMT(row, column, cellValue) {
      return this.subjectIDList[15 - cellValue]['label']
    },
    // 格式化题型
    questionTypeFMT(row, column, cellValue) {
      return this.questionTypeList[cellValue - 1]['label']
    },
    // 格式化难度
    difficultyFMT(row, column, cellValue) {
      return this.difficultyList[cellValue - 1]['label']
    },
    // 格式化审核状态
    chkStateFMT(row, column, cellValue) {
      if (cellValue === 0) {
        return '待审核'
      } else if (cellValue === 1) {
        return '通过'
      } else if (cellValue === 2) {
        return '拒绝'
      }
    },
    // 分页按钮
    changeCurrent(newpage) {
      this.searchForm.page = newpage
      this.getChoiceList(this.searchForm)
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
    // 精选题库
    this.getChoiceList()
  },
  // 事件监听
  watch: {
    activeName: function(val) {
      this.searchForm.chkState = val
    }
  }
}
</script>

<style scoped>
.button {
  background-color: #eee;
  color: #333;
  margin-bottom: 10px;
}
</style>
