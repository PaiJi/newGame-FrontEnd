<template lang="pug">
    el-row
        el-col
            el-table(:data="clubList")
                el-table-column(label="社团名称" prop="name" width="")
                el-table-column(label="社团简介" prop="intro" width="")
                el-table-column(label="社团人数" prop="members" width="")
                el-table-column(label="注册时间" prop="create_time" width="")
</template>
<script>
import axios from 'axios'
export default {
  data() {
    return {
      clubList: []
    }
  },
  mounted() {
    this.fillData()
  },
  methods: {
    async fillData() {
      console.log('Start GET Data')
      let { data } = await axios.get('/api/club/clublist')
      console.log(data)
      this.clubList = data
    }
  }
}
</script>
