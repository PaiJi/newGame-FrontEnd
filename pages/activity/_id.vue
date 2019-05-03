<template lang="pug">
    el-container
        el-header(style="height:auto;")
            myHeader
        el-main
            section.clubQuickBar
                el-row(:gutter='20')
                    el-col(:span='8')
                        .clubCoverImg(:style="{backgroundImage: `url(`+clubDetail.imgUrl+`)`}")
                    el-col(:span='16')
                        .clubTitle
                            h3 {{clubDetail.name}}
                        .clubInfo
                            el-row
                                el-col(:span='2')
                                    span.title 活动起始时间:
                                el-col(:span='16')
                                    span {{clubDetail.start_time}} —— {{clubDetail.end_time}}
                            el-row
                                el-col(:span='2')
                                    span.title 最大参与人数:
                                el-col(:span='16')
                                    span {{clubDetail.max_people}}
                            el-row
                                el-col(:span='2')
                                    span.title 活动类型:
                                el-col(:span='16')
                                    span {{clubDetail.type}}
                            el-row
                                el-col(:span='2')
                                    span.title 活动状态:
                                el-col(:span='16')
                                    span {{clubDetail.status}}
                            el-row
                                el-col(:span='2')
                                    span.title 签到要求:
                                el-col(:span='16')
                                    span {{clubDetail.enable_checkin}}
                            el-row
                                el-col(:span='2')
                                    span.title 活动简介:
                                el-col(:span='16')
                                    span {{clubDetail.intro}}
                        .clubPanel
                            el-row
                                el-col(:offset='2' :span='16')
                                    el-button(type="primary" @click="joinActivity") 加入活动
</template>
<style lang="scss">
section.clubQuickBar,
section.clubActivity {
  margin: 30px;
  padding: 30px;
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
    async joinActivity() {
      let { data } = await axios.get(
        '/api/activity/joinactivity?activityId=' + this.paramValue
      )
      console.log(data)
      if (data.queryResult === '1') {
        //弹窗告知用户已经加入
        this.$notify({
          title: '操作成功',
          message: '成功报名该活动，请记得按时参与哦',
          type: 'success'
        })
      }
      if (data.queryResult === '0') {
        //告诉用户社团需要邀请，加入失败
        this.$notify.error({
          title: '操作失败',
          message: data.errMsg
        })
      }
    }
  }
}
</script>
