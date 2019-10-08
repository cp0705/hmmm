<template>
  <div class="dashboard-container">
    <el-card>
      <el-row>
        <el-button type="info" size="mini">新增面试技巧</el-button>
        <el-row style="margin:10px 0" type="flex" justify="space-between">
          <el-col>
            <span class="key">关键字</span>
            <el-input
              placeholder="请输入题目编号/题干"
              v-model="keyword"
              size="mini"
              style="width:150px;margin-left: -5px;"
            ></el-input>
          </el-col>
          <el-col>
            <el-row type="flex" justify="end">
              <el-button size="mini">清除</el-button>
              <el-button size="mini" type="primary">搜索</el-button>
            </el-row>
          </el-col>
        </el-row>
      </el-row>
      <el-table :data="tableData" border style="width: 100%">
        <el-table-column type="index" width="50" label="序号"></el-table-column>
        <el-table-column prop="title" label="标题" width="400"></el-table-column>
        <el-table-column prop="visits" label="阅读数"></el-table-column>
        <el-table-column prop="state" label="状态" :formatter="statusFormatter"></el-table-column>
        <el-table-column prop="creator" label="录入人"></el-table-column>
        <el-table-column prop label="操作" width="250">
          <template slot-scope="obj">
            <el-button type="text">预览</el-button>
            <el-button type="text">修改</el-button>
            <el-button type="text">删除</el-button>
            <el-button type="text">{{obj.row.state?'禁用':'启用'}}</el-button>
          </template>
        </el-table-column>
      </el-table>
    </el-card>
  </div>
</template>

<script>
import { list } from '@/api/hmmm/articles'
export default {
  name: 'ArticlesList',
  data() {
    return {
      keyword: '',
      tableData: []
    }
  },
  methods: {
    clickxx(obj) {
      console.log(obj)
    },
    statusFormatter(row, column, cellValue, index) {
      return cellValue ? '启用' : '禁用'
    },
    async getArticles() {
      const { data: res } = await list()
      console.log(res)
      this.tableData = res.items
    }
  },
  created() {
    this.getArticles()
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
</style>
