<template>
  <div class="dashboard-container">
    <el-card>
      <div class="app-container">组题列表</div>
      <el-table :data="tableData" border style="width: 100%">
        <el-table-column type="index" width="50" label="序号"></el-table-column>
        <el-table-column prop="addTime" label="组题时间"></el-table-column>
        <el-table-column prop="userName" label="用户名"></el-table-column>
        <el-table-column prop="questionIDs" label="试题ID"></el-table-column>
        <el-table-column prop="progressOfAnswer" label="答题进度"></el-table-column>
        <el-table-column prop="accuracyRate" label="正确率"></el-table-column>
        <el-table-column prop="totalSeconds" label="答题总耗时(秒)"></el-table-column>
        <el-table-column prop="questionType" label="组题类型/详情"></el-table-column>
        <el-table-column prop label="操作">
          <template slot-scope="obj">
            <el-button type="text" @click="del(obj)">删除</el-button>
          </template>
        </el-table-column>
      </el-table>
        
      <!-- <el-row type="flex" justify="end" style="margin:20px 0">
        <el-pagination
        background
        @size-change="onPageSizeChange"
        @current-change="onPageChange"
        :current-page="pagination.page"
        :total="pagination.total"
        :page-size="pagination.pagesize"
        :page-sizes="[10,20,30, 50]"
        layout="sizes, prev, pager, next, jumper"
      ></el-pagination>
      </el-row> -->
      
    </el-card>
  </div>
</template>

<script>
import { randoms, removeRandoms } from '@/api/hmmm/questions'
export default {
  name: 'QuestionsRandoms',
  data() {
    return {
      tableData: [],
      pagination: {
        page: 1,
        total: 1000,
        pagesize: 20
      }
    }
  },
  created() {
    this.getQuestionsList()
  },
  methods: {
    // onPageChange(current) {
    //   this.pagination.page = current
    //   this.getQuestionsList()
    // },
    // onPageSizeChange(val) {
    //   this.pagination.pagesize = val
    //   this.getQuestionsList()
    // },
    async del(obj) {
      console.log(obj.row)
      var b = await removeRandoms(obj.row)
      if (b.statusText === 'OK') this.$message.success('删除成功')
      console.log(b) 
      this.getQuestionsList()
    },
    async getQuestionsList() {
      // console.log({page: this.pagination.page, pagesize: this.pagination.pagesize})
      const { data: res } = await randoms()
      console.log(res)
      this.tableData = res.items   
      this.pagination.total = res.counts
    }
  }
}
</script>
