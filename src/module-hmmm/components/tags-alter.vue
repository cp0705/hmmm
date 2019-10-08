<template>
<div>
   <div>
       <!-- 修改数据以后，子组件向父组件传递数据$emit -->
        标签名称：<el-input v-model="tagName.tagName"></el-input>
   </div>
   <div slot="footer">
       <el-button  @click="newTagName">取消</el-button>
       <el-button  @click="newTagName">确定</el-button>

   </div>
</div>
</template>

<script>

export default {
    name: 'TagsAlter',
    props: ['tagName'],
    methods: {
        newTagName() {
            this.$emit('newTagName', this.tagName.tagName, this.tagName.id)
        }   
    }
}
</script>

<style scoped>

</style>
