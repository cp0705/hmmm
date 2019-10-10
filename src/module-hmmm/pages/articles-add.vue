<template>
  <el-card>
    <el-form label-width="60px" ref="myform" :model="formData" :rules="rules">
      <el-form-item label="标题" prop="title">
        <el-input style="width:600px" placeholder="请输入文章标题" v-model="formData.title"></el-input>
      </el-form-item>
      <el-form-item prop="articleBody">
        <el-input
          type="textarea"
          rows="6"
          style="width:600px"
          placeholder="在此处输入文章正文"
          v-model="formData.articleBody"
        ></el-input>
      </el-form-item>
      <el-form-item label="视频地址" label-width="80px" prop="videoURL">
        <el-input style="width:580px" placeholder="请输入视频地址" v-model="formData.videoURL"></el-input>
      </el-form-item>
      <el-form-item style="margin-left:200px">
        <el-button type="primary" size="mini" @click="submitData">提交</el-button>
        <el-button size="mini" @click="toList">取消</el-button>
      </el-form-item>
    </el-form>
  </el-card>
</template>

<script>
import { add, detail, update } from '@/api/hmmm/articles'
export default {
  data() {
    return {
      rules: {
        title: [{ required: true, message: '标题不能为空' }],
        articleBody: [{ required: true, message: '正文不能为空' }],
        videoURL: [{ required: true, message: '视频地址不能为空' }]
      },
      formData: {
        title: '',
        articleBody: '',
        videoURL: ''
      }
    }
  },
  created() {
    const { id } = this.$route.params
    id && this.getDetailData(id)
  },
  methods: {
    async getDetailData(id) {
      const { data: res } = await detail({ id: id })
      this.formData = res
    },
    toList() {
      console.log(this.$router)
      this.$router.push('/articles/list')
    },
    submitData() {
      console.log(this.$router)

      let { id } = this.$route.params
      this.$refs.myform.validate(async isOk => {
        if (isOk) {
          if (id) {
            await update(this.formData)
          } else {
            await add(this.formData)
          }
          this.$router.push('/articles/list')
        }
      })
    }
  }
}
</script>

<style>
</style>
