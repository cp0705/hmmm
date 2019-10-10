<template>
  <div class="dashboard-container">
    <div class="app-container">
        <el-row >
          <!-- 学科选择框 -->
          <el-col :span="4">
            <el-row>
              <el-col  :span="4">
            <span class="bgspan">学科</span>
              </el-col>
              <el-col  :span="18">
            <el-select placeholder="请输入" size="mini" class="que-select"></el-select>
              </el-col>
            </el-row>
          </el-col> 
          <!-- 难度选择框 -->
          <el-col :span="4">
            <el-row>
              <el-col :span="4">
            <span class="bgspan">难度</span>
              </el-col>
              <el-col :span="18">
            <el-select placeholder="请输入" size="mini"  class="que-select"></el-select>
            </el-col>
           </el-row>
          </el-col>
          <!-- 试题类型选择框 -->
          <el-col :span="5">
            <el-row>
              <el-col :span="6">
            <span class="bgspan">试题类型</span>
              </el-col>
              <el-col :span="16">
            <el-select placeholder="请输入" size="mini"  class="que-select" ></el-select>
              </el-col>
          </el-row>
          </el-col>
       
          <!-- 标签选择框 -->
           <el-col :span="4">
           <el-row>
             <el-col :span="4">
            <span class="bgspan">标签</span>
             </el-col>
             <el-col :span="18">
            <el-select placeholder="请输入" size="mini"  class="que-select" ></el-select>
          </el-col>
           </el-row>
          </el-col>
          <el-col :span="6" >
            <el-row> 
                <el-col :span="3">
                  <span class="bgspan">城市</span>
                </el-col>
                <el-col :span="10">
              <el-select size="mini">
              </el-select>
              </el-col>
                <el-col :span="1" align="center">
                  <span>-</span>
              </el-col>
                <el-col :span="10">
              <el-select size="mini">
              </el-select>
              </el-col>
              </el-row>
          </el-col>
        </el-row>
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
.bgspan{
  font-size:12px;
}

</style>
