<template>
  <div class="dashboard-container">
    <div class="app-container">
      <el-card>
        <el-row>
          <el-button type='primary'>新增试题</el-button>
          <el-button type='danger'>批量导入</el-button>
        </el-row>
        <el-row>
          <el-col :span='6'>
            <span>城市：</span>
            <el-select></el-select>
          </el-col>
          <el-col :span='6'>
            <span>难度：</span>
            <el-select></el-select>
          </el-col>
          <el-col :span='6'>
            <span>试题类型：</span>
            <el-select></el-select>
          </el-col>
          <el-col :span='6'>
            <span>标签：</span>
            <el-select></el-select>
          </el-col>
        </el-row>
        <el-row>
          <el-col :span='6'>
            <span>城市：</span>
            <el-select></el-select>
            <el-select></el-select>
          </el-col>
          <el-col>
            <span></span>
          </el-col>
        </el-row>
      </el-card>
    </div>
  </div>
</template>

<script>
export default {
  name: 'QuestionsList',
  data() {
    return {}
  }
}
</script>

<style scoped>
</style>
