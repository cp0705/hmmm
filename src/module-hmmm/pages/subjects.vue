
<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card>
        <el-button
          style="color:#666;background:#f3f3f3;border-color:#ccc"
          size="mini"
          @click="openOrclose=true"
        >新增学科</el-button>
        <!--新增学科弹窗 -->
        <el-card
          body-style="padding:0px"
          style="z-index:11111;width:591px;margin:0 auto;position:fixed;left:0;right:0;margin:auto;"
          v-if="openOrclose"
        >
          <div slot="header" style="position:relative">
            <span style="font-size:14px;font-weight:600;color:#666">新增学科</span>
            <i
              @click="openOrclose=false"
              style="font-size:16px;font-weight:600;color:#666;position:absolute;right:20px;bottom:-10px;color:#ccc"
              class="el-icon-close"
            ></i>
          </div>
          <el-form
            :model="ruleForm"
            style="margin:20px;padding-bottom:20px"
            ref="ruleForm"
            label-width="100px"
            class="demo-ruleForm"
          >
            <el-form-item label="学科名称：" size="mini">
              <el-input placeholder="请输入" style="width: 320px;" :value="ruleForm.subjectName"></el-input>
            </el-form-item>
            <el-form-item label="是否显示：">
              <el-switch v-model="ruleForm.isFrontDisplay"></el-switch>
            </el-form-item>
          </el-form>
          <div style="border-top:2px solid #f3f3f3;height:60px;position:relative">
            <div style="position:absolute;right:20px;bottom:10px;">
              <el-button type="primary" size="mini">确定</el-button>
              <el-button size="mini" @click="openOrclose=false">取消</el-button>
            </div>
          </div>
        </el-card>
        <!-- 学科列表 -->
        <el-row type="flex" style="margin-top:10px;position:relative">
          <span
            style="display:inline-block;width:57px;height:27px;font-size:12px;background:#f3f3f3;line-height:27px;padding-left:5px;"
          >学科名称</span>
          <input
            type="text"
            style="border-radius:4px;border:1px solid #ccc;font-size:12px;padding-left:5px;width:180px;"
            placeholder="请输入"
          />
          <div style="position:absolute;right:40px;">
            <el-button size="mini">取消</el-button>
            <el-button type="primary" size="mini">搜索</el-button>
          </div>
        </el-row>
        <el-table :data="tableData" border style="width: 100%;font-size:12px;margin-top:10px;">
          <el-table-column type="index" label="序号" align="center" width="50"></el-table-column>
          <el-table-column prop="subjectName" label="学科名称" width="100"></el-table-column>
          <el-table-column label="创建者" width="100">
            <template slot-scope="obj">{{obj.row.creatorID|getCreator}}</template>
          </el-table-column>
          <el-table-column prop="addDate" :formatter="getTime" label="创建日期" width="150"></el-table-column>
          <el-table-column  label="前台是否显示" width="100">
            <template slot-scope="obj">{{obj.row.isFrontDisplay|Display}}</template>
          </el-table-column>
          <el-table-column prop="twoLevelDirectory" label="二级目录" width="100"></el-table-column>
          <el-table-column prop="tags" label="标签" width="100"></el-table-column>
          <el-table-column prop="totals" label="题目数量" width="100"></el-table-column>
          <el-table-column label="操作" min-width="100">
            <el-button style="font-size:12px" @click="$router.push('/subjects/directorys')" type="text">学科分类</el-button>
            <el-button style="font-size:12px" @click="$router.push('/subjects/tags')" type="text">学科标签</el-button>
            <el-button style="font-size:12px" type="text">修改</el-button>
            <el-button style="font-size:12px" type="text">删除</el-button>
          </el-table-column>
        </el-table>
        <el-row type="flex" justify="end" style="margin-top:20px">
          <el-pagination
            :page-size="pages.pagesize"
            :current-page="pages.page"
            background
            layout="prev, pager, jumper"
            :total="pagetotal"
            @current-change="changePage"
          ></el-pagination>
        </el-row>
      </el-card>
     
    </div>
  </div>
</template>

<script>
import { list } from '../../api/hmmm/subjects'
import { parseTime } from '../../filters/index'
import baseData from '../../api/base/baseApi'
export default {
  name: 'SubjectsList',
  data() {
    return {
      dialogVisible: false,
      openOrclose: false,
      ruleForm: {
        subjectName: '',
        isFrontDisplay: false
      },
      pages: {
        page: 1,
        pagesize: 10
      },
      pagetotal: 0,
      tableData: []
    }
  },
  methods: {
    async changePage(n) {
      this.pages.page = n
      var Data = await list(this.pages)
      this.tableData = Data.data.items
    },
    async getSubject() {
      var Data = await list()
      console.log(Data)
      this.tableData = Data.data.items
      this.pages.pagesize = Data.data.pagesize
      this.pagetotal = Data.data.counts
    },
    getTime(row, column, cellvalue, index) {
      return parseTime(cellvalue)
    }
  },
  filters: {
    getCreator(value) {
      var vada = ''
      baseData.role.map((va, i, arr) => {
        if (value === parseInt(va.id)) {
          vada = va.value
        }
      })
      return vada
    },
    Display(value) {
    switch (value) {
      case 1:  
        return '显示'
      case 2:
        return '隐藏'
    }
    }
  },
  created() {
    this.getSubject()
  }
}
</script>

<style scoped>
.creatsubject {
  background: #f3f3f3;
  width: 81px;
  height: 28px;
  font-size: 12px;
  line-height: 28px;
}
.el-input__inner {
  padding: 0;
  padding-left: 5px;
}
</style>
