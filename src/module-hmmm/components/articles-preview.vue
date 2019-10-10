<template>
  <div>
    <h3>{{dataObj.title}}</h3>
    <div>
      <span>{{dataObj.creatorID | showUser(userList)}}</span>
      <span>
        <i class="el-icon-view"></i>{{dataObj.visits}}
      </span>
    </div>
    <el-divider />
    <div>{{dataObj.articleBody}}</div>
  </div>
</template>

<script>
import { detail } from '@/api/hmmm/articles'

export default {
  props: ['dataId', 'userList'],
  data() {
    return {
      dialogVisible: false,
      dataObj: {}
    }
  },
  created() {
    this.getData()
  },
  filters: {
    showUser(val, arr) {
     return val ? arr.filter(item => item.id === val)[0].username : ''
    }
  },
  methods: {
    
    async getData() {
      const { data: res } = await detail({ id: this.dataId })
      this.dataObj = res
    }
  }
}
</script>

<style scoped>
h3 {
  font-weight: normal;
  font-size: 18px;
}
span {
  margin-right: 10px;
}
i {
  margin-right: 5px;
}
</style>
