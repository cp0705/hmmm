<template>
  <div class="dashboard-container">
    <el-card>
      <el-row>
        <el-button type="info" size="mini">新增试题</el-button>
        <el-button type="info" size="mini">批量导入</el-button>
      </el-row>
      <el-row :gutter="20">
        <el-col :span="4">
          <span class="key">学科</span>
          <el-select
            clearable
            v-model="formDate.subjectID"
            placeholder="请选择"
            size="mini"
            style="width:80%;margin-left: -5px;"
          >
            <el-option
              v-for="item in subjectsList"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-col>
        <el-col :span="4">
          <span class="key">难度</span>
          <el-select
            v-model="formDate.difficulty"
            placeholder="请选择"
            size="mini"
            style="margin-left: -5px;width:80%"
          >
            <el-option
              v-for="item in difficultyList"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-col>
        <el-col :span="4">
          <span class="key">试题类型</span>
          <el-select
            v-model="formDate.questionType"
            placeholder="请选择"
            size="mini"
            style="margin-left: -5px;width:60%"
          >
            <el-option
              v-for="item in questionTypeList"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-col>
        <el-col :span="4">
          <span class="key">标签</span>
          <el-select
            v-model="formDate.tags"
            placeholder="请选择"
            size="mini"
            style="margin-left: -5px;width:80%"
          >
            <el-option
              v-for="item in tagsList"
              :key="item.value"
              :label="item.label"
              :value="item.value"
            ></el-option>
          </el-select>
        </el-col>
        <el-col :span="8">
          <span class="key">城市</span>
          <el-select
            @change="getCityList"
            v-model="formDate.province"
            placeholder="城市"
            size="mini"
            style="margin-left: -5px;width:35%"
          >
            <el-option v-for="item in provincesList()" :key="item" :label="item" :value="item"></el-option>
          </el-select>
          <el-select v-model="formDate.city" placeholder="区县" size="mini" style="width:35%">
            <el-option v-for="item in cityList" :key="item" :label="item" :value="item"></el-option>
          </el-select>
        </el-col>
      </el-row>
      <el-row :gutter="15">
        <el-col :span="6">
          <span class="key">关键字</span>
          <el-input
            placeholder="请输入题目编号/题干"
            v-model="formDate.keyword"
            size="mini"
            style="width:80%;margin-left: -5px;"
          ></el-input>
        </el-col>
        <el-col :span="4">
          <span class="key">题目注释</span>
          <el-input
            placeholder="请输入"
            v-model="formDate.remarks"
            size="mini"
            style="width:66%;margin-left: -5px;"
          ></el-input>
        </el-col>
        <el-col :span="4">
          <span class="key">企业简称</span>
          <el-input
            placeholder="请输入"
            v-model="formDate.shortName"
            size="mini"
            style="width:66%;margin-left: -5px;"
          ></el-input>
        </el-col>
        <el-col :span="4">
          <span class="key">方向</span>
          <el-select
            v-model="formDate.direction"
            placeholder="请选择"
            size="mini"
            style="margin-left: -5px;width:80%"
          >
            <el-option v-for="item in direction" :key="item" :label="item" :value="item"></el-option>
          </el-select>
        </el-col>
        <el-col :span="4">
          <span class="key">录入人</span>
          <el-select
            v-model="formDate.creatorID"
            placeholder="请选择"
            size="mini"
            style="margin-left: -5px;width:70%"
          >
            <el-option
              v-for="item in userList"
              :key="item.id"
              :label="item.username"
              :value="item.id"
            ></el-option>
          </el-select>
        </el-col>
      </el-row>
      <el-row type="flex" justify="space-between">
        <el-col :span="4">
          <span class="key">二级目录</span>
          <el-input
            placeholder="请输入二级目录"
            v-model="formDate.catalogID"
            size="mini"
            style="width:66%;margin-left: -5px;"
          ></el-input>
        </el-col>
        <el-col :span="4">
          <el-row type="flex" justify="end" style="margin-top: 0px;">
            <el-button size="mini" @click="clearFormDate">清除</el-button>
            <el-button size="mini" type="primary" @click="getQuestionsList">搜索</el-button>
          </el-row>
        </el-col>
      </el-row>
      <el-table :data="questionsList" style="width: 100%" border>
        <el-table-column label="序号" type="index"></el-table-column>
        <el-table-column label="试题编号" prop="number"></el-table-column>
        <el-table-column label="学科" prop="subjectID"></el-table-column>
        <el-table-column label="题型" prop="questionType" :formatter="formatterQT"></el-table-column>
        <el-table-column label="题干" prop="question"></el-table-column>
        <el-table-column label="录入时间" prop="addDate">
          <template slot-scope="obj">{{ obj.row.addDate | parseTimeByString('{y}-{m}-{d}') }}</template>
        </el-table-column>
        <el-table-column label="难度" prop="difficulty" :formatter="formatterDiff"></el-table-column>
        <el-table-column label="录入人" prop="creator"></el-table-column>
        <el-table-column label="操作" width="250">
          <template slot-scope="obj">
            <el-button type="text">预览</el-button>
            <el-button type="text">修改</el-button>
            <el-button type="text" @click="del(obj.row)">删除</el-button>
            <el-button type="text">加入精选</el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-card>
  </div>
</template>

<script>
import { provinces as provincesList, citys } from '@/api/hmmm/citys'
import { simple as userList } from '@/api/base/users'
import { list, remove } from '@/api/hmmm/questions'
import { simple } from '@/api/hmmm/subjects'
import { simple as simpleTags } from '@/api/hmmm/tags'
import {
  difficulty as difficultyList,
  questionType as questionTypeList,
  direction
} from '@/api/hmmm/constants'
import { async } from 'q'
export default {
  name: 'QuestionsList',
  data() {
    return {
      questionsList: [],
      subjectsList: [],
      difficultyList,
      questionTypeList,
      tagsList: [],
      cityList: [],
      direction,
      userList: [],

      formDate: {
        subjectID: '', // 学科
        difficulty: '', // 难度
        questionType: '', // 题型
        tags: '', // 标签
        province: '', // 省市
        city: '', // 区县
        keyword: '', // 关键字
        remarks: '', // 注释
        shortName: '', // 企业简称
        direction: '', // 方向
        creatorID: '', // 录入人
        catalogID: '' // 二级目录
      }
    }
  },
  created() {
    this.getQuestionsList()
    this.getSubjectList()
    this.getTages()
    this.getUserList()
  },
  methods: {
    clearFormDate() {
      this.formDate = {}
      this.getQuestionsList()
    },
    async getUserList() {
      const { data: res } = await userList()
      this.userList = res
    },
    async getCityList(pname) {
      console.log(pname)
      this.formDate.city = ''
      this.cityList = citys(pname)
    },
    provincesList,
    async getTages() {
      const { data: res } = await simpleTags()
      this.tagsList = res
    },
    async getSubjectList() {
      const { data: res } = await simple()
      this.subjectsList = res
    },
    async del(obj) {
      try {
        await this.$confirm('确定删除吗?', '删除', {
          confirmButtonText: '确定',
          cancelButtonText: '取消',
          type: 'warning'
        })
        await remove(obj)
        this.$message.success('删除成功')
        this.getQuestionsList()
      } catch (err) {}
    },
    formatterQT(r, c, cc) {
      return this.questionTypeList[cc - 1]['label']
    },
    formatterDiff(r, c, cc) {
      return this.difficultyList[cc - 1]['label']
    },
    async getQuestionsList() {
      const { data: res } = await list(this.formDate)
      this.questionsList = res.items
    }
  }
}
</script>

<style scoped>
span.key {
  display: inline-block;
  background: #eee;
  height: 28px;
  line-height: 28px;
  vertical-align: middle;
  padding: 0 5px;
  font-size: 12px;
}
.el-row, .el-table {
  margin-top: 10px;
}
</style>
