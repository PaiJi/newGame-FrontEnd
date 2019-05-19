<template lang="pug">
    el-container
        el-header(style="height:auto;")
            myHeader
        el-main
            section.container(v-if="showError==false" v-cloak v-loading="showError")
                el-row(:gutter='20' type="flex" justify="center")
                    el-col(:sm='10' :md='10')
                        .clubCoverImg(:style="{backgroundImage: `url(`+activityDetail.imgUrl+`)`}")
                el-row(:gutter='20' type="flex" justify="center")
                    el-col(:sm='10' :md='10')
                        .clubTitle
                            h3 {{activityDetail.name}}
                el-row(:gutter='20' type="flex" justify="center")
                    el-col(:sm='10' :md='10')
                        span 活动时间：{{activityDetail.start_time}} — {{activityDetail.end_time}}
                el-row(:gutter='20' type="flex" justify="center" style="margin-top:10px")
                    el-col(:sm='10' :md='10')
                        span.title 活动详情：{{activityDetail.intro}}
                el-row(:gutter='20' type="flex" justify="center" style="margin-top:50px")
                    el-col(:sm='10' :md='10')
                        el-button(type="primary" @click="checkin" style="width:100%") 签到
            section.showErr(v-if="showError")
                el-row(type="flex" justify='center')
                    el-col(:sm='10' :md='10')
                        el-alert(type="error" effect="dark" title="抱歉，读取活动信息时似乎遇到了问题。")
                el-row(type="flex" justify='center').errImg
                    el-col(:sm='10' :md='10')
                        <img src="data:image/png;base64,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">
                el-row(type='flex' justify='center').errMsg
                    el-col(:sm='10' :md='10')
                        h4 如您频繁看到此界面，请联系管理员。
                        h4 5秒后我们会将您重定向到首页。
</template>
<style lang="scss">
[v-cloak] {
  display: none;
}
.container {
  margin: 0 auto;
  padding: 40px 0px;
  max-width: 1200px;
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
section.showErr {
  height: 80vh;
  .errImg,
  .errMsg {
    margin: 20px 0px;
    text-align: center;
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
      activityDetail: [],
      showError: false,
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
      if (data != null) {
        this.activityDetail = data
      } else {
        this.showError = true
        setTimeout(() => {
          this.$router.push({ path: '/' })
        }, 5000)
      }
    },
    async checkin() {
      let { data } = await axios.get(
        '/api/activity/checkinactivity?activityId=' + this.paramValue
      )
      //console.log(data)
      if (data.queryResult === '1') {
        this.$notify({
          title: '签到成功',
          message: '签到成功，感谢参加本次活动！',
          type: 'success'
        })
      }
      if (data.queryResult === '0') {
        this.$notify.error({
          title: '签到未成功',
          message: data.errMsg
        })
      }
    }
  }
}
</script>
