<template>
  <div class="dashboard-container">
    <el-card>
      <el-row>
        <el-button type="info" size="mini" @click="$router.push('/articles/add')">新增面试技巧</el-button>
        <el-row style="margin:10px 0" type="flex" justify="space-between">
          <el-col>
            <span class="key">关键字</span>
            <el-input
              placeholder="请输入题目编号/题干"
              v-model="page.keyword"
              size="mini"
              style="width:150px;margin-left: -5px;"
            ></el-input>
          </el-col>
          <el-col>
            <el-row type="flex" justify="end">
              <el-button size="mini" @click="page.keyword = '';getArticles()">清除</el-button>
              <el-button size="mini" type="primary" @click="getArticles">搜索</el-button>
            </el-row>
          </el-col>
        </el-row>
      </el-row>
      <el-table :data="tableData" border>
        <el-table-column type="index" width="180" label="序号"></el-table-column>
        <el-table-column prop="title" label="标题" ></el-table-column>
        <el-table-column prop="visits" label="阅读数"></el-table-column>
        <el-table-column prop="state" label="状态" :formatter="statusFormatter"></el-table-column>
        <el-table-column prop="creator" label="录入人"></el-table-column>
        <el-table-column prop label="操作">
          <template slot-scope="obj">
            <el-button type="text" @click="lookArticle(obj)">预览</el-button>
            <el-button type="text" @click="$router.push(`/articles/add/${obj.row.id}`)">修改</el-button>
            <el-button type="text" @click="del(obj)">删除</el-button>
            <el-button type="text" @click="changeStatus(obj)">{{obj.row.state?'禁用':'启用'}}</el-button>
          </template>
        </el-table-column>
      </el-table>
      <!-- 文章详情弹框 -->
      <el-dialog :visible="dialogVisible" @close="dialogVisible=false" title="文章详情">
        <articles-preview :dataId="lookArticleId" :userList="userList"></articles-preview>
      </el-dialog>
      <el-row type="flex" justify="end" style="margin:20px 0">
        <el-pagination
        background
        @size-change="onPageSizeChange"
        @current-change="onPageChange"
        :current-page="page.page"
        :total="page.total"
        :page-size="page.pagesize"
        :page-sizes="[10,20,30, 50]"
        layout="sizes, prev, pager, next, jumper"
      ></el-pagination>
      </el-row>
    </el-card>
  </div>
</template>

<script>
import { list, state, remove } from '@/api/hmmm/articles'
import { simple } from '@/api/base/users'
import articlesPreview from '@/module-hmmm/components/articles-preview'
export default {
  name: 'ArticlesList',
  data() {
    return {
      userList: [],
      lookArticleId: '',
      dialogVisible: false,
      keyword: '',
      tableData: [],
      page: {
        page: 1,
        pagesize: 10,
        keyword: '',
        total: 10
      }
    }
  },
  components: {
    'articles-preview': articlesPreview
  },
  methods: {
    onPageChange(current) {
      this.page.page = current
      this.getArticles()
    },
    onPageSizeChange(val) {
      this.page.pagesize = val
      this.getArticles()
    },
    async getUserList() {
      const { data: res } = await simple()
      this.userList = res
    },
    lookArticle(obj) {
      console.log(typeof obj.row.id)
      this.dialogVisible = true
      this.lookArticleId = obj.row.id
    },
    async del(obj) {
      try {
        await this.$confirm('确定删除吗')
        await remove({ id: obj.row.id })
        this.$message.success('删除成功')
        this.getArticles()
      } catch (error) {}
    },
    async changeStatus(obj) {
      await state({ id: obj.row.id, state: Number(!obj.row.state) })
      this.getArticles()
    },
    statusFormatter(row, column, cellValue, index) {
      return cellValue ? '启用' : '禁用'
    },
    async getArticles() {
      const { data: res } = await list(this.page)
      this.tableData = res.items
      this.page.total = res.counts
      console.log(res)
    }
  },
  created() {
    this.getArticles()
    this.getUserList()
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
/* el-table-column[prop="title"] {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
} */
</style>
