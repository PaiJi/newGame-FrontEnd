<template lang="pug">
    .container
        el-row(:gutter="20")
            el-col(:span="8")
                el-card
                    h5 我的社团
                    span {{myClubList.length}}
            el-col(:span="8")
                el-card
                    h5 我的活动
                    span {{myActivityList.length}}
            //el-col(:span="8")
                el-card
                    h5 
                    span COMING SOON
        el-row.indexCarousel
            el-col
                el-carousel( :interval="4000")
                    el-carousel-item
                        img(:style="{backgroundImage:'url(https://storge-1251771711.cos.ap-shanghai.myqcloud.com/visitor-resouces/img/15.jpg)'}")

</template>
<style lang="scss" scoped>
.indexCarousel {
  margin: 20px 0px;
  .el-carousel__item {
    img {
      width: 100%;
      height: 300px;
      background-size: cover;
    }
  }
}
</style>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      myClubList: [],
      myActivityList: []
    }
  },
  mounted() {
    this.getUserClubList()
    this.getUserActivityList()
  },
  methods: {
    async getUserClubList() {
      let { data } = await axios.get('/api/club/getmyclublist')
      //console.log(data)
      if (data.getMyClubListResultCode == '0') {
        //console.log('LOGIN!')
      }
      if (data.queryResult == '1') {
        this.myClubList = data.data
        //console.log(this.myClubList.length)
      }
    },
    async getUserActivityList() {
      let { data } = await axios.get('/api/activity/getmyactivity')
      if (data.queryResult == '1') {
        this.myActivityList = data.queryData
      }
    }
  }
}
</script>
