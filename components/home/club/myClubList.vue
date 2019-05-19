<template lang="pug">
    .container
        .clubList(v-if="whichIsShow=='list'")
            el-row
                el-col
                    el-table(:data="clubList" @current-change="showClubDetailPanel")
                        el-table-column(label="社团名称" prop="clubInfo.name" width="")
                        el-table-column(label="社团简介" prop="clubInfo.intro" width="")
                        el-table-column(label="社团人数" prop="clubInfo.members" width="")
                        el-table-column(label="注册时间" prop="clubInfo.create_time" width="")
        .clubDetailContainer(v-if="whichIsShow=='clubPanel'")
            el-row.clubStatusBar
                el-col(:span='4')
                    el-button(@click="whichIsShow='list'") 返回列表
                el-col(:span='20')
                    h4 {{selectClub.name}}
                    span 详细信息
        .clubPanel(v-if="whichIsShow=='clubPanel'")
            el-tabs(type="border-card")
                el-tab-pane(label="社团详情").clubDetail
                    el-row
                        el-col(:md="4")
                            p 社团名称
                        el-col(:md="20")
                           p {{selectClub.name}}
                    el-row
                        el-col(:md="4")
                            p 社团简介
                        el-col(:md="20")
                            p {{selectClub.intro}}
                    el-row
                        el-col(:md="4")
                            p 社团状态
                        el-col(:md="20")
                            p {{selectClub.status}}
                    el-row
                        el-col(:md="4")
                            p 社团成员数
                        el-col(:md="20")
                            p {{selectClub.members}}
                    el-row
                        el-col(:md="4")
                            p 社团类别
                        el-col(:md="20")
                            p {{selectClub.sort}}
                    el-row
                        el-col(:md="4")
                            p 社团所属部门
                        el-col(:md="20")
                            p {{selectClub.belong}}
                    el-row
                        el-col(:md="4")
                            p 社团创建时间
                        el-col(:md="20")
                            p {{selectClub.create_time}}
                    el-row
                        el-col(:md="4")
                            p 社团加入模式
                        el-col(:md="20")
                            p {{selectClub.join_mode}}
                    el-row
                        el-col(:md="4")
                            p 社团ID
                        el-col(:md="20")
                            p {{selectClub.id}}
                el-tab-pane(label="社团活动")
                        el-table(:data='clubActivityList', style='width: 100%')
                            el-table-column(label='活动名称', width='180')
                                template(slot-scope='scope')
                                    i.el-icon-time
                                    span(style='margin-left: 10px') {{ scope.row.name }}
                            //- el-table-column(label='活动简介', width='180')
                            //-     template(slot-scope='scope')
                            //-         span {{ scope.row.intro }}
                            el-table-column(label='活动类型' width='180')
                                template(slot-scope='scope')
                                    .name-wrapper(slot='reference')
                                        el-tag(size='medium') {{ scope.row.type }}
                            el-table-column(label='活动状态' width='180')
                                template(slot-scope='scope')
                                    .name-wrapper(slot='reference')
                                        el-tag(size='medium') {{ scope.row.status }}
                            el-table-column(label='开始时间' width='180')
                                template(slot-scope='scope')
                                    span(style='margin-left: 10px') {{ scope.row.start_time }}
                            el-table-column(label='结束时间' width='180')
                                template(slot-scope='scope')
                                    span(style='margin-left: 10px') {{ scope.row.end_time }}
                            el-table-column(label='操作')
                                template(slot-scope='scope')
                                    el-button(size='mini', type='success', @click='joinActivity(scope.row.id)') 加入
                el-tab-pane(label="社团通讯录")
                    el-table(:data="clubContact")
                        el-table-column(label="姓名" prop="username" width="")
                        el-table-column(label="昵称" prop="nickname" width="")
                        el-table-column(label="手机号码" prop="phone" width="")
                        el-table-column(label="角色" prop="clubRole" width="")


</template>
<script>
import axios from 'axios'
export default {
  data() {
    return {
      clubList: [],
      selectClub: [],
      clubActivityList: [],
      clubContact: [],
      whichIsShow: 'list'
    }
  },
  mounted() {
    this.fillData()
  },
  methods: {
    async fillData() {
      let { data } = await axios.get('/api/club/getmyclublist')
      //console.log(data)
      if (data.getMyClubListResultCode == '0') {
        this.$notify.error({
          title: '操作失败',
          message: '系统错误信息：' + data.errMsg
        })
      }
      if (data.queryResult == '1') {
        this.clubList = data.data
      }
    },
    async showClubDetailPanel(val) {
      console.log(val)
      this.selectClub = val.clubInfo
      let { data } = await axios.get(
        '/api/club/getclubcontact?clubid=' + this.selectClub.id
      )
      this.clubContact = data
      this.getActivityData(val)
      this.whichIsShow = 'clubPanel'
    },
    async getActivityData(val) {
      let { data } = await axios.get(
        '/api/activity/activityOfClub?clubId=' + val.clubInfo.id
      )
      this.clubActivityList = data
    },
    async joinActivity(Id) {
      let { data } = await axios.get(
        '/api/activity/joinactivity?activityId=' + Id
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
<style lang="scss" scoped>
.clubDetail {
  .el-row {
    border-bottom: solid 1px rgba(240, 240, 240, 1);
  }
  .el-col-md-4 {
    color: gray;
  }
}
.clubDetailContainer {
  .clubStatusBar {
    border-radius: 4px;
    box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.1);
    margin: 20px 0px;
    padding: 20px 20px;
    margin-left: 0px;
  }
  .el-row {
    border-bottom: none;
  }
  .el-col-md-4 {
    color: gray;
  }
}
</style>
