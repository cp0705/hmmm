<template>
  <div class="dashboard-container">
    <div class="app-container">
      <!-- 子向父传递数据$emit -->

        标签名称：<el-input v-model="newTag" style="width: 300px"></el-input>
        <div>
          
        学科：<el-select v-model="value">
                  <el-option v-for="(item,index) in subjectList" :key="index" :value="item.value" :label="item.label"></el-option>
             </el-select>
             </div>
    </div>
    <div slot="footer">
        <el-button @click="close(1)">取 消</el-button>
        <el-button type="primary" @click="close(2)">确 定</el-button>
    </div>
  </div>
</template>
<script>
import {simple} from '@/api/hmmm/subjects.js'
export default {
  name: 'TagsAdd',
  data() {
    return {
      newTag: '',
      subjectList: [],
      value: ''
    }
  },
  methods: {
    close(value) {
       this.$emit('newtag', this.newTag, this.value, value)
       this.newTag = ''
       this.value = ''
    },
    // 获取科目的简单列表
    async getSubjectList() {
      // 对象数组，label和value
       let res = await simple()
       this.subjectList = res.data

    }
  },
  created() {
    this.getSubjectList()
  }
}
</script>

<style scoped>
.app-container .el-input{
    margin-bottom: 30px;
  }
 
</style>
