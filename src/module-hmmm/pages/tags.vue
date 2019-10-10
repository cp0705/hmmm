<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card>
        <el-row>
          <el-button type="info" plain @click="OpenDialog">新建标签</el-button>
          <el-dialog :visible="dialogVisible" title="新建标签">
            <!-- 接收到了tags-add传递的数据 -->
            <tag-add @newtag="newTagNames"></tag-add>
          </el-dialog>
          <el-button type="info" plain @click="goSeject">返回学科</el-button>
        </el-row>
        <el-row class="tageNAME">
          标签名称：
          <el-input style="width: 300px;height: 20px" v-model="name" @change="selectTage(name)"></el-input>
        </el-row>
        <el-table :data="tableData" width="100%">
          <el-table-column label="序号" type="index" width="180px"></el-table-column>
          <el-table-column label="标签名称" prop="tagName"></el-table-column>
          <el-table-column label="创建者" prop="username"></el-table-column>
          <el-table-column label="创建日期">
            <span slot-scope="obj">{{obj.row.addDate | parseTimeByString}}</span>
          </el-table-column>
          <el-table-column label="面试题数量" prop="totals"></el-table-column>
          <el-table-column label="状态" prop="state" :formatter="stateForm"></el-table-column>
          <el-table-column label="操作">
            <template slot-scope="obj">
              <el-button type="text" @click="alterTagName(obj.row)">修改</el-button>
              <el-button type="text" @click="disabledTagName(obj.row)">禁用</el-button>
              <el-button type="text" @click="deleteTagName(obj.row.id)">删除</el-button>
            </template>
          </el-table-column>
        </el-table>
        <!-- 修改的对话框 -->
        <el-dialog :visible="dialogVisible2" title="修改标签">
          <!-- 接收到了子组件tags-alter传递的数据 -->
          <!-- 向子组件传递标签名和id -->
          <tag-alter :tagName="modForm" @newTagName="getTagName"></tag-alter>
        </el-dialog>
      </el-card>
    </div>
  </div>
</template>

<script>
import { status } from '@/api/hmmm/constants.js'
import { update, list, add, remove, removeState } from '@/api/hmmm/tags.js'
import TagAdd from '@/module-hmmm/components/tags-add.vue'
import TagAlter from '@/module-hmmm/components/tags-alter.vue'
import { parseTimeByString } from '@/filters/index.js'
export default {
  name: 'TagsList',
  data() {
    return {
      modForm: null, // 修改表单对象
      tableData: [],
      formData: {
        id: null,
        subjectName: null,
        tagName: null,
        username: null,
        addDate: null,
        totals: null,
        state: null
      },
      dialogVisible: false,
      dialogVisible2: false,
      newTagName: '', // 新增加的标签名
      name: '' // 搜索框输入的标签名
    }
  },
  methods: {
    // 搜索
    async selectTage(name) {
      this.formData.tagName = name
      await this.getTagList(this.formData)
      this.name = ''
    },

    // 状态禁用
    async disabledTagName(row) {
      await removeState(row)
      await this.getTagList()
    },
    // 删除
    async deleteTagName(id) {
      await this.$confirm('您确定要删除吗?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'warning'
      })
      await remove({ id: id })

      await this.getTagList()
    },
    // 状态的格式
    stateForm(row, column, cellValue) {
      // return status[cellValue]['label']
      return cellValue ? '启动' : '禁用'
    },
    // 获取新的标签值,给相对应的列
    async getTagName(newTN, id) {
      // this.newTagName = value
      this.dialogVisible2 = false
      // 调用接口，修改数据
      let data = { subjectID: id, id: id, tagName: newTN }

      await update(data)
      await this.getTagList()
    },
    // 更改标签名称
    alterTagName(row) {
      // 获取相应的id

      // console.log(row)
      this.modForm = row
      this.dialogVisible2 = true
    },
    // 获取表单数据
    async getTagList() {
      let result = await list(this.formData)
      this.tableData = result.data.items
    },
    // 打开弹窗
    OpenDialog() {
      this.dialogVisible = true
    },
    // 新增标签名
    async newTagNames(tagname, subjectID, value) {
      this.newTagName = tagname
      this.dialogVisible = false
      if (value === 2) {
        let data = { tagName: this.newTagName, subjectID }
        await add(data)
        await this.getTagList()
      }
    },
    // 跳转管理
    goSeject() {
      this.$router.push('/subjects/list')
    }
  },
  components: {
    'tag-add': TagAdd,
    'tag-alter': TagAlter
  },
  created() {
    this.getTagList()
  }
}
</script>

<style scoped>
.tageNAME {
  margin-top: 30px;
  margin-bottom: 30px;
}
</style>
