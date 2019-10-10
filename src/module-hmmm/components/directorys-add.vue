<template>
  <div class="dashboard-container">
    <div class="app-container">
      
    </div>
  </div>
</template>

<script>
// import { add, list } from '@/api/hmmm/directorys'

export default {
  name: 'DirectorysAdd',
  data() {
    return {
      
    }
  },
  methods: {
    
    async getList() {
      let result = await list()
      this.lists = result.data.items
    }
    
  }
}
</script>

<style scoped>
</style>
