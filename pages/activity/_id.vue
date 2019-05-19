<template lang="pug">
    el-container
        el-header(style="height:auto;")
            myHeader
        el-main
            section.clubQuickBar(v-if="showError==false" v-cloak v-loading="showError")
                el-row(:gutter='20')
                    el-col(:md="10" :lg="10")
                        .clubCoverImg(:style="{backgroundImage: `url(`+clubDetail.imgUrl+`)`}")
                    el-col(:md="14" :lg="14")
                        .clubTitle
                            h2 {{clubDetail.name}}
                        .clubInfo
                            el-form
                                el-form-item(label="活动起始时间:")
                                    span {{clubDetail.start_time}} —— {{clubDetail.end_time}}
                                el-form-item(label="最大参与人数:")
                                    span {{clubDetail.max_people}}
                                el-form-item(label="活动类型:")
                                    span {{clubDetail.type}}
                                el-form-item(label="活动状态:")
                                    span {{clubDetail.status}}
                                el-form-item(label="签到要求:")
                                    span {{clubDetail.enable_checkin}}
                                el-form-item(label="活动介绍:").longTextKill
                                    span {{clubDetail.intro}}
                                el-form-item.join_button
                                    el-button(type="primary" @click="joinActivity") 加入活动
            section.activityDetail(v-if="showError==false" v-cloak v-loading="showError")
                h4 活动详情
                p {{clubDetail.intro}}
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
section.activityDetail {
  margin: 20px 0px;
  padding: 20px;
  h4 {
    margin-bottom: 20px;
  }
  p {
    color: #606266;
  }
  background-color: #ebeef5;
  border-radius: 5px;
}
section.showErr {
  height: 80vh;
  .errImg,
  .errMsg {
    margin: 20px 0px;
    text-align: center;
  }
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
