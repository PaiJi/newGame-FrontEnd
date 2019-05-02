<template lang="pug">
    el-container
        el-header(style="height:auto;")
            myHeader
        el-main
            section.clubQuickBar
                el-row(:gutter='20' type="flex" justify="center")
                    el-col(:sm='4' :md='10')
                        .clubCoverImg(:style="{backgroundImage: `url(`+clubDetail.imgUrl+`)`}")
                el-row(:gutter='20' type="flex" justify="center")
                    el-col(:sm='4' :md='10')
                        .clubTitle
                            h3 {{clubDetail.name}}
                el-row(:gutter='20' type="flex" justify="center")
                    el-col(:sm='4' :md='10')
                        span 活动时间：{{clubDetail.start_time}} — {{clubDetail.end_time}}
                el-row(:gutter='20' type="flex" justify="center" style="margin-top:10px")
                    el-col(:sm='4' :md='10')
                        span.title 活动详情：{{clubDetail.intro}}
                el-row(:gutter='20' type="flex" justify="center" style="margin-top:50px")
                    el-col(:sm='4' :md='10')
                        el-button(type="primary" @click="checkin" style="width:100%") 签到
                    //- el-col(:span='16')
                    //-     .clubTitle
                    //-         h3 {{clubDetail.name}}
                    //-     .clubInfo
                    //-         el-row
                    //-             el-col(:span='2')
                    //-                 span.title 活动起始时间:
                    //-             el-col(:span='16')
                    //-                 span {{clubDetail.start_time}} —— {{clubDetail.end_time}}
                    //-         el-row
                    //-             el-col(:span='2')
                    //-                 span.title 所属:
                    //-             el-col(:span='16')
                    //-                 span {{clubDetail.belong}}
                    //-         el-row
                    //-             el-col(:span='2')
                    //-                 span.title 最大参与人数:
                    //-             el-col(:span='16')
                    //-                 span {{clubDetail.max_people}}
                    //-         el-row
                    //-             el-col(:span='2')
                    //-                 span.title 活动类型:
                    //-             el-col(:span='16')
                    //-                 span {{clubDetail.type}}
                    //-         el-row
                    //-             el-col(:span='2')
                    //-                 span.title 活动状态:
                    //-             el-col(:span='16')
                    //-                 span {{clubDetail.status}}
                    //-         el-row
                    //-             el-col(:span='2')
                    //-                 span.title 社团简介:
                    //-             el-col(:span='16')
                    //-                 span {{clubDetail.intro}}
                    //-     .clubPanel
                    //-         el-row
                    //-             el-col(:offset='2' :span='16')
                    //-                 el-button(type="primary" @click="joinClub") 加入活动
</template>
<style lang="scss">
section.clubQuickBar,
section.clubActivity {
  margin: 30px;
  //padding: 30px;
}
.clubCoverImg {
  width: 100%;
  height: 365px;
  background-size: cover;
  border-radius: 5px;
  display: inline-block;
}
.clubInfo {
  margin: 20px 0px;
  .el-row {
    margin: 10px 0px;
  }
  .el-col-2 {
    margin-right: 10px;
  }
  span.title {
    color: #99a2aa;
    font-size: 14px;
    //margin-right: 15px;
  }
}
.clubPanel {
  .el-button {
    width: 200px;
  }
}
section.clubActivity {
  //background-color: ;
  .el-col {
    background-color: #f4f5f7;
    padding: 20px;
    border-radius: 5px;
  }
}
</style>

<script>
import axios from 'axios'
import myHeader from '~/components/header.vue'
export default {
  components: {
    myHeader
  },
  data() {
    return {
      paramValue: '',
      clubDetail: [],
      userId: myHeader.data().userId
    }
  },
  mounted() {
    this.paramValue = this.$route.params.id
    this.getActivityDetail()
  },
  methods: {
    async getActivityDetail() {
      let { data } = await axios.get(
        '/api/activity/activityDetail?activityId=' + this.paramValue
      )
      this.clubDetail = data
    },
    async checkin() {
      let { data } = await axios.get(
        '/api/activity/checkinactivity?activityId=' + this.paramValue
      )
      console.log(data)
      if (data.queryResult === '1') {
        this.$notify({
          title: '签到成功',
          message: '签到成功，感谢参加本次活动！',
          type: 'success'
        })
      }
      if (data.queryResult === '0') {
        //告诉用户社团需要邀请，加入失败
        this.$notify.error({
          title: '签到未成功',
          message: data.errMsg
        })
      }
    }
  }
}
</script>
