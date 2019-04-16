<template lang="pug">
    .container
        el-row(:gutter="20")
            el-col(:span="8")
                el-card
                    h5 我的社团
                    span 5
            el-col(:span="8")
                el-card
                    h5 我的活动
                    span 2
            el-col(:span="8")
                el-card
                    h5 设计中
                    span COMING SOON
        el-row
            el-col
                el-carousel(type="card" :interval="4000")
                    el-carousel-item
                    el-carousel-item
                    el-carousel-item

</template>
<script>
import axios from 'axios'
export default {
  data() {
    return {
      myClubList: []
    }
  },
  mounted() {
    this.fillData()
  },
  methods: {
    async fillData() {
      console.log('Start GET Data')
      let { data } = await axios.get('/api/club/getmyclublist')
      console.log(data)
      if (data.getMyClubListResultCode == '0') {
        console.log('LOGIN!')
      }
      if (data.queryResult == '1') {
        this.myClubList = data.queryData
        console.log(this.myClubList.length)
      }
    }
  }
}
</script>
