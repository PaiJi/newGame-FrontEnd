<template lang="pug">
    el-container
        el-header(style="height:auto;")
            myHeader
        el-main
            section.clubQuickBar
                el-row(:gutter='20')
                    el-col(:span='8')
                        .clubCoverImg(:style="{backgroundImage: `url(`+clubDetail.img_logo+`)`}")
                    el-col(:span='16')
                        .clubTitle
                            h3 {{clubDetail.name}}
                        .clubInfo
                            el-row
                                el-col(:span='2')
                                    span.title 创立时间:
                                el-col(:span='16')
                                    span {{clubDetail.create_time}}
                            el-row
                                el-col(:span='2')
                                    span.title 所属:
                                el-col(:span='16')
                                    span {{clubDetail.belong}}
                            el-row
                                el-col(:span='2')
                                    span.title 现有成员:
                                el-col(:span='16')
                                    span {{clubDetail.members}}
                            el-row
                                el-col(:span='2')
                                    span.title 加入模式:
                                el-col(:span='16')
                                    span {{clubDetail.join_mode}}
                            el-row
                                el-col(:span='2')
                                    span.title 当前状态:
                                el-col(:span='16')
                                    span {{clubDetail.status}}
                            el-row
                                el-col(:span='2')
                                    span.title 社团简介:
                                el-col(:span='16')
                                    span {{clubDetail.intro}}
                        .clubPanel
                            el-row
                                el-col(:offset='2' :span='16')
                                    el-button(type="primary" @click="joinClub") 立刻加入
            section.clubActivity
                el-row(:gutter="20")
                    el-col(:span="15")
                        h5 康康这个社团最近的大新闻
                    el-col(:offset='2' :span="6")
                        h5 放个公告，管理员信息
                el-row(:gutter="20")
                    el-col(:span="15")
                        p 抓鸽子大赛顺利举行！
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
    this.getClubDetail()
  },
  methods: {
    async getClubDetail() {
      let { data } = await axios.get(
        '/api/club/getclubdetail?clubid=' + this.paramValue
      )
      this.clubDetail = data
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
          message: data.errMsg
        })
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
    }
  }
}
</script>
