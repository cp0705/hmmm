<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card shadow="never">
        <el-form :model="addForm" ref="addForm" :rules="rules">
          <!-- 学科 -->
          <el-form-item label='学科:' prop="subjectID" >
            <el-select style="width:450px" v-model="addForm.subjectID" clearable>
              <el-option v-for="item in subjectIDList" :key="item.value"
              :value="item.value" :label="item.label"></el-option>
            </el-select>
          </el-form-item>
          <!-- 目录 -->
          <el-form-item label='目录:' prop="catalogID">
            <el-select style="width:450px" v-model="addForm.catalogID" clearable>
              <el-option v-for="item in directorysList" :key="item.value"
                    :value="item.value" :label="item.label"></el-option>
            </el-select>
          </el-form-item>
          <!-- 企业 -->
          <el-form-item label='企业:' prop="enterpriseID">
            <el-select style="width:450px" v-model="addForm.enterpriseID" clearable>
              <el-option v-for="item in enterpriseList" :key="item.id"
              :value="item.id" :label="item.company"></el-option>
            </el-select>
          </el-form-item>
          <!-- 城市 -->
          <el-form-item label='城市:' required>
            <!-- 省份 -->
            <el-col :span="5">
              <el-form-item prop="province">
                <el-select style="width:220px;margin-right:7px" v-model="addForm.province" @change="getCitysList" clearable>
                  <el-option v-for="item in provincesList" :key="item" :value="item" :label="item"></el-option>
                </el-select>
              </el-form-item>
            </el-col>
            <!-- 区县 -->
            <el-col :span="5">
              <el-form-item prop="city">
                <el-select style="width:220px" v-model="addForm.city" clearable>
                  <el-option v-for="item in citysList" :key="item" :value="item" :label="item"></el-option>
                </el-select>
              </el-form-item>
            </el-col>
          </el-form-item>
          <!-- 方向 -->
          <el-form-item label='方向:' prop='direction' >
            <el-select style="width:450px" v-model="addForm.direction" clearable>
              <el-option v-for="item in directionList" :key="item"
                    :value="item" :label="item"></el-option>
            </el-select>
          </el-form-item>
          <!-- 题型 -->
          <el-form-item label='题型:' prop="questionType" >
            <el-radio-group v-model="addForm.questionType">
              <el-radio v-for="item in questionTypeList" :key="item.value"
              :label="item.value+''">{{item.label}}</el-radio>
            </el-radio-group>
          </el-form-item>
          <!-- 难度 -->
          <el-form-item label='难度:' prop="difficulty" >
            <el-radio-group v-model="addForm.difficulty">
              <el-radio v-for="item in difficultyList" :key=" item.value"
              :label="item.value+''">{{item.label}}</el-radio>
            </el-radio-group>
          </el-form-item>
          <!-- 题干 -->
          <el-form-item label='题干:' prop="question">
            <el-input type="textarea" v-model="addForm.question"
            :rows="6" placeholder="请输入内容"></el-input>
          </el-form-item>
          <!-- 选项 (以下选中的选项为正确答案) -->
          <el-form-item label='选项 (以下选中的选项为正确答案)' v-if="allShow"
          v-model="addForm.options"><br>
          <!-- 单选 -->
            <template v-if="radioShow">
              <el-radio v-model="singleSelect" :label="0" style="margin-bottom:15px">
                A： <el-input type="text" v-model="addForm.options[0]['title']"></el-input>
              </el-radio><br />
              <el-radio v-model="singleSelect" :label="1" style="margin-bottom:15px">
                B：<el-input type="text" v-model="addForm.options[1]['title']"></el-input>
              </el-radio><br />
              <el-radio v-model="singleSelect" :label="2" style="margin-bottom:15px">
                C：<el-input type="text" v-model="addForm.options[2]['title']"></el-input>
              </el-radio><br />
              <el-radio v-model="singleSelect" :label="3">
                D： <el-input type="text" v-model="addForm.options[3]['title']"></el-input>
              </el-radio>
            </template>
            <!-- 多选 -->
            <template v-else>
              <el-checkbox v-model="addForm.options[0]['isRight']" style="margin-bottom:15px">
                A：<el-input type="text" v-model="addForm.options[0]['title']"></el-input>
              </el-checkbox>
              <br>
              <el-checkbox v-model="addForm.options[1]['isRight']" style="margin-bottom:15px">
                B：<el-input type="text" v-model="addForm.options[1]['title']"></el-input>
              </el-checkbox>
              <br>
              <el-checkbox v-model="addForm.options[2]['isRight']" style="margin-bottom:15px">
                C：<el-input type="text" v-model="addForm.options[2]['title']"></el-input>
              </el-checkbox>
              <br>
              <el-checkbox v-model="addForm.options[3]['isRight']" >
                D：<el-input type="text" v-model="addForm.options[3]['title']"></el-input>
              </el-checkbox>
            </template>
          </el-form-item>
          <!-- 解析视频 -->
          <el-form-item label='解析视频:' prop="videoURL">
            <input type="text" placeholder="请输入视频地址，以http(s)://开头" 
            style="width:250px" v-model="addForm.videoURL">
          </el-form-item>
          <!-- 答案解析： -->
          <el-form-item label='答案解析:' prop="answer"><br>
            <el-input type="textarea" v-model="addForm.answer"
            :rows="6" placeholder="请输入内容"></el-input>
          </el-form-item>
          <!-- 题目备注： -->
          <el-form-item label='题目备注:' prop="remarks">
            <el-input type="textarea" v-model="addForm.remarks"
            :rows="6" placeholder="请输入内容"></el-input>
          </el-form-item>
          <!-- 试题标签 -->
          <el-form-item label='试题标签:' prop="tags">
            <el-input style="width:250px" placeholder="请输入" v-model="addForm.tags"></el-input>
          </el-form-item>
        </el-form>
        <!-- 提交按钮 -->
        <el-row style="text-align:center">
          <el-button type="primary" @click="submitForm('addForm')">提交</el-button>
          <el-button >取消</el-button>
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
import {direction as directionList,
        questionType as questionTypeList,
        difficulty as difficultyList
      } from '@/api/hmmm/constants'// 方向
import {list} from '@/api/hmmm/companys'// 企业
import {add} from '@/api/hmmm/questions'
export default {
  name: 'QuestionsNew',
  data() {
    return {
      // 单选或者多选
      radioShow: true, // 默认显示单选项目
      allShow: true, // 单选或多选默认显示一个
      // 校验规则
      rules: {
        subjectID: [{required: true, message: '请选择学科', trigger: 'change'}],
        catalogID: [{required: true, message: '请选择目录', trigger: 'change'}],
        enterpriseID: [{required: true, message: '请选择企业', trigger: 'change'}],
        city: [{required: true, message: '请选择城市', trigger: 'change'}],
        province: [{required: true, message: '请选择区县', trigger: 'change'}],
        direction: [{required: true, message: '请选择方向', trigger: 'change'}],
        questionType: [{required: true, message: '请选择题型', trigger: 'change'}],
        difficulty: [{required: true, message: '请选择难度', trigger: 'change'}],
        question: [{required: true, message: '请填写题干', trigger: 'blur'}],
        // options: [{message: '请选择正确答案', trigger: 'change'}],
        videoURL: [{message: '请填写视频地址', trigger: 'change'}],
        answer: [{required: true, message: '请填写答案解析', trigger: 'blur'}],
        remarks: [{message: '请填写题目备注', trigger: 'blur'}],
        tags: [{required: true, message: '请填写标签', trigger: 'blur'}]
      },
      subjectIDList: [], // 学科列表
      provincesList: [], // 省份
      citysList: [], // 城区
      directionList, // 方向
      enterpriseList: [], // 企业
      directorysList: [], // 目录
      questionTypeList, // 题型
      difficultyList, // 难度
      singleSelect: '', // 选项
        // 搜索表单
      addForm: {
        subjectID: '', // 学科 
        catalogID: '', // 目录
        enterpriseID: '', // 企业
        city: '', // 企业所在城市
        province: '', // 企业所在城市
        direction: '', // 方向
        questionType: '1', // 试题类型
        difficulty: '1', // 难度
        question: '', // 题干
        options: [ // 选项
          { code: 'A', title: '', img: '', isRight: false },
          { code: 'B', title: '', img: '', isRight: false },
          { code: 'C', title: '', img: '', isRight: false },
          { code: 'D', title: '', img: '', isRight: false }
        ],
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
      this.addForm.city = '' // 清除之前选取好的城市
      var res = await citys(pname)
      this.citysList = res
    },
    // 企业
    async getenterpriseList() {
      var res = await list()
      this.enterpriseList = res.data.items
    },
    // 表单验证+数据提交完成填写题库
    submitForm(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            // 收集表单数据，完成添加操作
            // 注意：要使得数据完成添加后，再做列表页面重定向操作
            add(this.addForm) // 添加数据
            this.$message.success('添加数据成功')
            this.$router.push('/questions/choice')// 路由重定向跳转
          } else {
            console.log('error submit!!')
            return false
          }
        })
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
  },
   // watch监听器
  watch: {
    // 监听选项选中要把isRight的值做设置操作
    singleSelect(newval) {
      // 要使得全部的isRight变为false
      // newval代表的当前项目的isRight变为true
      for (var i = 0; i < 4; i++) {
        this.addForm.options[i].isRight = false
      }
      this.addForm.options[newval].isRight = true
    },
    // 对题型进行监听
    'addForm.questionType': function(newval) {
      // console.log(newval)
      if (Number(newval) === 1) {
        // newval=1 单选
        this.radioShow = true
        this.allShow = true
      } else if (Number(newval) === 2) {
        // newval=2 多选
        this.radioShow = false
        this.allShow = true
      } else {
        // newval=3 简答
        this.allShow = false
      }
    }
  }
}
</script>

<style scoped>
</style>
