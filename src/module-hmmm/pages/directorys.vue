<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card>
        <el-row>
          <el-button type='primary' @click="dialogVisible = true">新建目录</el-button>
          <el-dialog :visible.sync="dialogVisible">
            <span><p>新建目录</p></span>
            <span slot="footer">
              <el-button @click="dialogVisible = false">取 消</el-button>
              <el-button type="primary" @click="dialogVisible = false;addDirectory()">确 定</el-button>
            </span>
            <el-row>
              <span>学科类别：</span>
              <el-select v-model="formData.subjectID" clearable>
                <el-option v-for='item in disciplineList' :key='item.label' 
                :label="item.label"
                :value="item.value"></el-option>
              </el-select>
            </el-row>
            <el-row style="margin-top:20px">
              <span>目录添加：</span>
              <el-input style='width:300px' v-model="formData.directoryName"></el-input>
            </el-row>
          </el-dialog>
          <el-button type='primary' @click="goDiscipline">返回学科</el-button>
        </el-row>
        <el-row class='search'>
          <div class="catagory">
            <span>目录名称</span>
            <el-input placeholder="请输入内容" style="width:200px" v-model="formData.directoryName"></el-input>
          </div>
          <div class="state">
            <span>状态</span>
            <el-select v-model="formData.state" clearable>
              <el-option v-for='item in status' :key='item.value' 
              :label="item.label"
              :value="item.value"></el-option>
            </el-select>
            <el-button style="margin-left:8px" @click="cleanWord">清除</el-button>
            <el-button type='primary' @click='getStatus'>搜索</el-button>
          </div>
        </el-row>
        <el-table
          :data="lists"
          border
          style="width: 100%">
          <el-table-column
            type='index'
            label="序号"
            width="80"
            align='center'>
          </el-table-column>
          <el-table-column
            prop="directoryName"
            label="目录名称"
            width="190"
            align='center'>
          </el-table-column>
          <el-table-column
            prop="username"
            label="创建者"
            width='190'
            align='center'>
          </el-table-column>
          <el-table-column
            prop="addDate"
            :formatter='changeTime'
            label="创建日期"
            width='200'
            align='center'>
          </el-table-column>
          <el-table-column
            prop="totals"
            label="面试题数量"
            width='190'
            align='center'>
          </el-table-column>
          <el-table-column
            label="状态"
            width='160'
            align='center'>
            <template slot-scope="scope">
              <p>{{ scope.row.state | changeState }}</p>
            </template>
          </el-table-column>
          <el-table-column
            prop="operation"
            label="操作"
            align='center'>
            <template slot-scope="obj">
              <el-button size="small" @click="getDirectory(obj.row);dialogVisibleTwo=true">修改</el-button>
              <el-button size="small" @click="changeState(obj.row)">
                {{obj.row.state ? '禁用' : '启用'}}</el-button>
              <el-button size="small" @click="delDirectory(obj.row)">删除</el-button>
            </template>
          </el-table-column>
        </el-table>
        <el-dialog :visible.sync="dialogVisibleTwo">
          <span>修改目录</span>
            <span slot="footer">
              <el-button @click="dialogVisibleTwo = false">取 消</el-button>
              <el-button type="primary" @click="dialogVisibleTwo = false;updateDirectory()">确 定</el-button>
            </span>
            <el-row>
              <span>学科类别：</span>
              <el-select v-model="formData.subjectID" clearable>
                <el-option v-for='item in disciplineList' :key='item.label' 
                :label="item.label"
                :value="item.value"></el-option>
              </el-select>
            </el-row>
            <el-row style="margin-top:20px">
              <span>目录添加：</span>
              <el-input style='width:300px' v-model="formData.directoryName"></el-input>
            </el-row>
        </el-dialog>
        <el-row type='flex' justify='center' style="padding:15px 0">
          <el-pagination
            background
            layout="prev, pager, next"
            :total="pagination.total"
            @current-change='changePage'
            :page-size='pagination.pagesize'
            :current-page='pagination.page'>
          </el-pagination>
        </el-row>
        
      </el-card>
    </div>
  </div>
</template>

<script>
import { list, remove, update, add, removeState, detail } from '../../api/hmmm/directorys'
import { status } from '../../api/hmmm/constants'
import { parseTimeByString } from '../../filters/index'
import { simple } from '@/api/hmmm/subjects'
export default {
  name: 'DirectorysList',
  data() {
    return {
      dialogVisible: false, // 弹层
      dialogVisibleTwo: false,
      lists: [], // 目录列表
      formData: { // 请求的数据
        directoryName: null,
        state: null,
        subjectID: null
      },
      disciplineList: [], // 学科类别
      pagination: {
        page: 1,
        pagesize: 10,
        total: 0
      }
    }
  },
  methods: {
    // 返回学科
    goDiscipline() {
      this.$router.push({
        path: '/subjects/list'
      })
    },
    // 获取学科
    async getDiscipline() {
      let result = await simple()
      this.disciplineList = result.data
    },
    // 获取目录列表
    async getStatus() {
      let result = await list(this.formData)
      // debugger
      // console.log(this.formData.directoryName)
      this.lists = result.data.items
      this.pagination.total = result.data.counts
      // console.log(result)
    },
    async changePage(newPage) {
      // debugger
      this.pagination.page = newPage
      let result = await list(this.pagination)
      this.lists = result.data.items
    },
    // 添加学科
    async addDirectory() {
      await add(this.formData)
      this.cleanWord()
    },
    // 改变状态
    async changeState(row) {
      // debugger
      let mess = row.state ? '禁用' : '启用'
      await this.$confirm(`此操作将${mess}该目录, 是否继续?`, {
        type: 'warning'
      })
      let data = {id: row.id, state: Number(!row.state)}
      // debugger
      await removeState(data)
      // debugger
      this.getStatus()
      
    },
    // 清除输入框内容
    cleanWord() {
      this.formData.directoryName = null
      this.formData.state = null
      this.getStatus()
    },
    // 删除操作
    delDirectory(id) {
      this.$confirm('确认要删除该题库么?', '删除', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      }).then(() => {
        remove(id) // 删除数据
        this.getStatus() // 更新数据
      })
    },
    // 修改操作=> 获取目录详情
    async getDirectory(data) {
      // debugger
      // console.log(data.id)
      let result = await detail(data)
      
      this.formData = result.data
    },
    // 修改操作
    async updateDirectory() {
      await update(this.formData)
      this.cleanWord()
    },
    // 时间格式化
    changeTime(row, column, cellvalue, index) {
      // debugger
      return parseTimeByString(cellvalue)
    }
  },
  // 引入状态
  computed: {
    status() {
      return status
    }
  },
  // 使用过滤器改变状态
  filters: {
    changeState(value) {
      return value === 1 ? '启用' : '禁用'
    }
  },
  // 钩子函数
  created() {
    this.getStatus()
    // 获取学科
    this.getDiscipline()
  }
}
</script>

<style scoped>
.search{
  padding: 20px 0;
}
.search .catagory,.state{
  float:left
}
.state{
  margin-left: 10px;
}

</style>
