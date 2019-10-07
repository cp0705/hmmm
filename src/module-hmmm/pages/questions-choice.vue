<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card shadow="never">
        <!-- 按钮 -->
        <el-button class="button">新增试题</el-button>
        <el-button class="button">批量导入</el-button>
        <!-- 搜索栏目 -->
        <el-form>
            <el-row>
              <!-- 学科 -->
              <el-col :span='5'>
                <el-form-item label='学科'>
                  <el-select style='width:180px'>
                  </el-select>
                </el-form-item>
              </el-col>
              <!-- 状态 -->
              <el-col :span='5'>
                <el-form-item label='状态'>
                  <el-select style='width:180px'></el-select>
                </el-form-item>
              </el-col>
              <!-- 难度 -->
              <el-col :span='4'>
                <el-form-item label='难度'>
                  <el-select style='width:135px'></el-select>
                </el-form-item>
              </el-col>
              <!-- 试题类型 -->
              <el-col :span='5'>
                <el-form-item label='试题类型'>
                  <el-select style='width:150px'></el-select>
                </el-form-item>
              </el-col>
              <!-- 标签 -->
              <el-col :span='5'>
                <el-form-item label='标签'>
                  <el-input style='width:140px' placeholder="请输入"></el-input>
                </el-form-item>
              </el-col>
              <!-- 录入人 -->
              <el-col :span='4'>
                <el-form-item label='录入人'>
                  <el-input style='width:120px' placeholder="请输入"></el-input>
                </el-form-item>
              </el-col>
              <!-- 关键字 -->
              <el-col :span='6'>
                <el-form-item label='关键字'>
                  <el-input style='width:210px' placeholder="请输入题目编号/题干"></el-input>
                </el-form-item>
              </el-col>
              <!-- 题目备注 -->
              <el-col :span='4'>
                <el-form-item label='题目备注'>
                  <el-input style='width:110px' placeholder="请输入"></el-input>
                </el-form-item>
              </el-col>
              <!-- 城市 -->
              <el-col :span='5'>
                <el-form-item label='城市'>
                  <el-select style='width:180px'></el-select>
                </el-form-item>
              </el-col>
              <el-col :span='5'>
                <el-form-item>
                  <el-select style='width:180px'></el-select>
                </el-form-item>
              </el-col>
              <!-- 二级目录 -->
              <el-col :span='5'>
                <el-form-item label='二级目录'>
                  <el-input style='width:150px' placeholder="请输入二级目录"></el-input>
                </el-form-item>
              </el-col>
              <!-- 企业简称 -->
              <el-col :span='5'>
                <el-form-item label='企业简称'>
                  <el-input style='width:150px' placeholder="请输入"></el-input>
                </el-form-item>
              </el-col>
              <!-- 方向 -->
              <el-col :span='5'>
                <el-form-item label='方向'>
                  <el-input style='width:140px' placeholder="请输入"></el-input>
                </el-form-item>
              </el-col>
            </el-row>
            <!-- 标签选择 -->
            <template>
              <el-tabs type="card">
                <el-tab-pane label="全部" ></el-tab-pane>
                <el-tab-pane label="待审核"></el-tab-pane>
                <el-tab-pane label="已审核"></el-tab-pane>
              </el-tabs>
              <!-- 表格数据 -->
              <el-table border :header-cell-style="{
                'background-color': 'rgb(248, 248, 248)','color': '#555','height':'60px',
                'font-weight':'400'
              }">
                <el-table-column label='序号' width='50px' class="table"></el-table-column>
                <el-table-column label='试题编号' width='90px'></el-table-column>
                <el-table-column label='学科' width='80px'></el-table-column>
                <el-table-column label='题型' width='80px'></el-table-column>
                <el-table-column label='题干' width='150px'></el-table-column>
                <el-table-column label='录入时间' width='150px'></el-table-column>
                <el-table-column label='录入人'></el-table-column>
                <el-table-column label='难度' width='60px'></el-table-column>
                <el-table-column label='使用次数'></el-table-column>
                <el-table-column label='审核状态'></el-table-column>
                <el-table-column label='审核意见'></el-table-column>
                <el-table-column label='审核人'></el-table-column>
              </el-table>
            </template>
            <!-- 分页按钮 -->
            <el-pagination type='primary' style="text-align:center" background
              @size-change="handleSizeChange"
              @current-change="handleCurrentChange"
              :current-page.sync="currentPage3"
              :page-size="100"
              layout="prev, pager, next, jumper"
              :total="1000">
            </el-pagination>
        </el-form>
      </el-card>
    </div>
  </div>
</template>

<script>
export default {
  name: 'QuestionsChoice',
  data() {
    return {
    }
  },
  methods: {
    
  }
}
</script>

<style scoped>
.button{
  background-color: #eee;
  color: #333;
  margin-bottom: 10px
}
.table{
  background-color: #ccc;
  height: 100px;
  color:#000
  
}

</style>
