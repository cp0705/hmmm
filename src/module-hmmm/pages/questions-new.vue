<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card>
        <el-form ref="addFormRef" :model="addForm" label-width="120px">
          <!--:model作用：可以使得el-form针对表单的全部信息进行收集，固定属性-->
          <el-form-item label="学科：">
            <el-select v-model="addForm.subjectID">
              <el-option
                v-for="item in subjectIDList"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              ></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="目录：">
            <el-select v-model="addForm.catalogID">
              <el-option
                v-for="item in catalogIDList"
                :key="item.value"
                :label="item.label"
                :value="item.value"
              ></el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="企业：">
            <el-select v-model="addForm.enterpriseID">
              <el-option
               v-for="item in enterpriseIDList"
               :key="item.id"
               :label="item.company"
               :value="item.id">
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="城市：">
            <el-select v-model="addForm.province" @change="getCitys">
              <el-option 
                v-for="item in provinces()" 
                :key="item" 
                :label="item" 
                :value="item">
              </el-option>
            </el-select>
            <el-select v-model="addForm.city">
              <el-option 
               v-for="item in cityList" 
               :key="item" 
               :label="item" 
               :value="item">
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="方向：">
            <el-select v-model="addForm.direction">
              <el-option 
                v-for="item in directionList" 
                :key="item" 
                :value="item" 
                :label="item">
              </el-option>
            </el-select>
          </el-form-item>
          <el-form-item label="题型：">
            <el-radio-group v-model="addForm.questionType">
               <el-radio v-for="item in questionTypeList" :key="item.value" :label="item.value+''">
               {{item.label}}
               </el-radio>
            </el-radio-group>
          </el-form-item>
          <el-form-item label="难度：">
            <el-radio-group v-model="addForm.difficulty">
              <el-radio v-for="item in difficultyList" :key="item.value"     :label="item.value+''">
              {{item.label}}
              </el-radio>
            </el-radio-group>
          </el-form-item>
        </el-form>
      </el-card>
    </div>
  </div>
</template>

<script>
import { simple as subjectsSimple } from '@/api/hmmm/subjects' // 获取标签信息方法导入
import { simple as directorysSimple } from '@/api/hmmm/directorys' // 获取二级目录信息方法导入
import {provinces, citys} from '@/api/hmmm/citys' // 获取 省份/城市 信息方法导入
import { direction as directionList, questionType as questionTypeList, difficulty as difficultyList } from '@/api/hmmm/constants'// 获取方向信息导入  题型 单选按钮表单域  难度信息导入
import { list as companysList } from '@/api/hmmm/companys' // 企业
export default {
  name: 'QuestionsNew',
  data() {
    return {
      subjectIDList: [], // 学科下拉列表
      catalogIDList: [], // 目录下拉列表
      cityList: [], // 区县下拉列表
      directionList, // 方向下拉列表
      enterpriseIDList: [], // 企业下拉列表
      questionTypeList, // 题型简易赋值
      difficultyList, // 难度简易赋值
      // 添加试题，表单各项值
      addForm: {
        subjectID: '', // 学科
        catalogID: '', // 目录
        enterpriseID: '', // 企业
        province: '', // 城市
        city: '', // 区县
        direction: '', // 方向
        questionType: '1', // 默认选择单选，要求是字符串
        difficulty: '1' // 默认 难度 第一个项目被选中(要求是字符串)
      }
    }
  },
  created() {
    this.getSubjectIDList()// 获取学科
    this.getCatalogIDList()// 获取目录
        this.getEnterpriseIDList() // 获得企业
  },
  methods: {
    // 获得学科列表信息
    async getSubjectIDList() {
      var rst = await subjectsSimple()
      this.subjectIDList = rst.data
      // console.log(rst)
    },
    // 获得 录入人 列表数据
    async getCatalogIDList() {
      var rst = await directorysSimple()
      this.catalogIDList = rst.data
    },
    // 获得企业列表信息
    async getEnterpriseIDList() {
      var rst = await companysList()
      this.enterpriseIDList = rst.data.items
      console.log(rst)
    },
    // 获得 城市 信息，简易成员赋值
    provinces, // provinces:provinces
    // 获得 区县 信息
    // getCitys方法在模板中声明的时候，没有设置()
    // 这个pname形参就代表被选中的省份信息
    getCitys(pname) {
      this.addForm.city = '' // 清除之前选取好的城市
      this.cityList = citys(pname)
    }
  }
}
</script>

<style scoped>
</style>
