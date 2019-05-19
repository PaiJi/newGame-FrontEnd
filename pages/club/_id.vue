<template lang="pug">
    el-container
        el-header(style="height:auto;")
            myHeader
        el-main
            .container
                section.clubQuickBar
                    el-row(:gutter='20')
                        el-col(:md="10" :lg="10")
                            .clubCoverImg(:style="{backgroundImage: `url(`+clubDetail.img_logo+`)`}")
                        el-col(:md="14" :lg="14")
                            .clubTitle
                                h2 {{clubDetail.name}}
                            .clubInfo
                                el-form
                                    el-form-item(label="创立时间:")
                                        span {{clubDetail.create_time}}
                                    el-form-item(label="所属:")
                                        span {{clubDetail.belong}}
                                    el-form-item(label="现有成员:")
                                        span {{clubDetail.members}}
                                    el-form-item(label="加入模式:")
                                        span {{clubDetail.join_mode}}
                                    el-form-item(label="当前状态:")
                                        span {{clubDetail.status}}
                                    el-form-item(label="社团简介:").longTextKill
                                        span {{clubDetail.intro}}
                                    el-form-item.join_button
                                        el-button(type="primary" @click="joinClub") 立刻加入
                section.clubActivity
                    el-row
                        el-col(:md="16" :lg="16").clubActivityList
                            h4 社团活动
                            el-table(:data="clubActivityList" @row-click="openActivity" style="width:100%")
                                el-table-column(prop="name" label="活动名称")
                                el-table-column(prop="start_time" label="活动开始时间")
                        el-col(:md="8" :lg="8").clubIntro
                            h4 社团详情
                            p  {{clubDetail.intro}}
</template>
<style lang="scss">
.container {
  max-width: 1200px;
  margin: 0 auto;
}
.clubCoverImg {
  width: 100%;
  height: 365px;
  background-size: cover;
  border-radius: 5px;
  display: inline-block;
}
.clubInfo {
  margin: 10px 0px;
  .el-form-item {
    margin-bottom: 0px;
    .el-form-item__label {
      color: #99a2aa;
      //font-size: 14px;
    }
  }
  .longTextKill {
    span {
      text-overflow: ellipsis;
      white-space: nowrap;
      overflow: hidden;
      display: block;
      width: 75%;
    }
  }
  .join_button {
    .el-button {
      width: 200px;
      margin-top: 20px;
    }
  }
  @media (max-width: 700px) {
    .join_button {
      .el-button {
        width: 100%;
        margin-top: 20px;
      }
    }
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
    //background-color: #f4f5f7;
    padding: 20px;
    border-radius: 5px;
  }
  .clubIntro {
    background-color: #f4f5f7;
    border-radius: 0px;
    h4 {
      color: #303133;
      margin-bottom: 10px;
    }
    p {
      color: #606266;
    }
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
      clubActivityList: [],
      userId: myHeader.data().userId
    }
  },
  mounted() {
    this.paramValue = this.$route.params.id
    this.getClubDetail()
    this.getClubActivity()
  },
  methods: {
    async getClubDetail() {
      let { data } = await axios.get(
        '/api/club/getclubdetail?clubid=' + this.paramValue
      )
      this.clubDetail = data
    },
    async getClubActivity() {
      let { data } = await axios.get(
        '/api/activity/activityofclub?clubId=' + this.paramValue
      )
      this.clubActivityList = data
    },
    async joinClub() {
      let { data } = await axios.get(
        '/api/club/joinclub?clubid=' + this.paramValue
      )
      console.log(data)
      if (data.joinClubResultCode === '1') {
        //弹窗告知用户已经加入
        this.$notify({
          title: '成功加入社团辣',
          message: '您已经是社团成员了，快去看看吧！',
          type: 'success'
        })
      }
      if (data.joinClubResultCode === '0' && data.clubRequest === '1') {
        //告诉用户需要填表
        this.$notify.info({
          title: '需要填写表单',
          message: '正在将您重定向至表单页面'
        })
        this.$router.push({ path: '/club/apply/' + this.paramValue })
      }
      if (data.joinClubResultCode === '0' && data.clubRequest === '2') {
        //告诉用户社团需要邀请，加入失败
        this.$notify.error({
          title: '这个社团当前需要邀请',
          message: data.errMsg
        })
      }
      if (data.joinClubResultCode === '0' && data.clubRequest === '0') {
        //社团暂不纳新，加入失败
        this.$notify.error({
          title: '加入大失败',
          message: data.errMsg
        })
      }
      if (data.joinClubResultCode === '0' && data.code === '42') {
        this.$notify.error({
          title: '哎呀！',
          message: data.errMsg
        })
      }
    },
    openActivity(event) {
      console.log(event)
      this.$router.push({ path: '/activity/' + event.id })
    }
  }
}
</script>
