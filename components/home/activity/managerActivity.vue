<template lang="pug">
    .container
        .clubSelect
            el-row
                el-col(:span='6')
                    el-select(v-model="selectClub" placeholder="请选择你管理的社团")
                        el-option(v-for="item in clubList" :label="item.name" :value="item.id")
                el-col(:span='6')
                    el-button(@click="getActivityData") 查询活动
        .clubList(v-if="whichIsShow=='list'")
            el-row
                el-col
                    el-table(:data="activityList" @current-change="showActivityDetailPanel")
                        el-table-column(label="活动名称" prop="name" width="")
                        el-table-column(label="活动状态" prop="intro" width="")
                        el-table-column(label="活动开始时间" prop="start_time" width="")
                        el-table-column(label="活动结束时间" prop="end_time" width="")
        .clubDetailContainer(v-if="whichIsShow=='clubPanel'")
            el-row.clubStatusBar
                el-col(:span='4')
                    el-button(@click="whichIsShow='list'") 返回列表
                el-col(:span='20')
                    h4 {{selectActivity.name}}
                    span 详细信息
            .clubPanel
                el-tabs(type="border-card")
                    el-tab-pane(label="修改活动信息").clubDetail
                        el-form(:model="updateActivityForm" label-width="100px" :rules="rules" ref="ruleForm")
                            el-row
                                el-col(:md='20')
                                    el-form-item(label="活动名称" prop="name")
                                        el-input(v-model="updateActivityForm.name")
                            el-row
                                el-col(:md='20')
                                    el-form-item(label="海报地址" prop="imgUrl")
                                        el-input(v-model="updateActivityForm.imgUrl")
                            el-row
                                el-col(:md='20')
                                    el-form-item(label="活动简介" prop="intro")
                                        el-input(type="textarea" :row=4 placeholder="随便写点什么，让大家了解你的社团活动" v-model="updateActivityForm.intro")
                            el-row
                                el-col(:md='20')
                                    el-form-item(label="最大人数" prop="max_people")
                                        el-input(v-model="updateActivityForm.max_people" placeholder="无限制直接留空")
                            el-row
                                el-col(:md='20')
                                    el-form-item(label="活动开始时间" prop="start_time")
                                        el-date-picker(v-model="updateActivityForm.start_time" type="datetime" value-format="yyyy-MM-dd HH:mm:ss" placeholder="请选择活动开始时间")
                            el-row
                                el-col(:md='20')
                                    el-form-item(label="活动结束时间" prop="end_time")
                                        el-date-picker(v-model="updateActivityForm.end_time" type="datetime" value-format="yyyy-MM-dd HH:mm:ss" placeholder="请选择活动结束时间")
                            el-row
                                el-col
                                    el-form-item(label="活动类型" prop="type")
                                        el-radio-group(v-model="updateActivityForm.type")
                                            el-radio(label="1") 社内活动
                                            el-radio(label="2") 公开活动
                            el-row
                                el-col
                                    el-form-item(label="活动状态" prop="status")
                                        el-radio-group(v-model="updateActivityForm.status")
                                            el-radio(label="0") 未开始
                                            el-radio(label="1") 报名中
                            el-row
                                el-col
                                    el-form-item(label="签到功能" prop="enable_checkin")
                                        el-radio-group(v-model="updateActivityForm.enable_checkin")
                                            el-radio(label="0") 关闭
                                            el-radio(label="1") 开启
                            el-row
                                el-col
                                    el-form-item
                                        el-button(type="primary" @click="submitForm('ruleForm')") 更新信息
                                        el-button(type="danger" @click="deteleActivity") 删除活动
                    el-tab-pane(label="查看报名情况")
                        el-table(:data="activityApplyListData" style="width:100%")
                            el-table-column(label="姓名" prop="username" fixed)
                            el-table-column(label="昵称" prop="nickname")
                            el-table-column(label="性别" prop="gender")
                            el-table-column(label="专业" prop="major")
                            el-table-column(label="手机号" prop="phone")
                            el-table-column(label="QQ" prop="qq")
                            el-table-column(label="Wechat" prop="wechat")
                    el-tab-pane(label="签到列表")
                        .checkin-progress
                            el-row
                                el-col(:span='4')
                                    p 最大报名人数：{{selectActivity.max_people}}
                                el-col(:span='4')
                                    p 实际报名人数：
                                el-col(:span='4')
                                    p 已签到人数：
                            el-row
                                el-col
                                    h4 签到情况：
                            el-row
                                el-col
                                    el-progress(type="circle" :percentage="25")
                        .checin-table
                            el-table(:data="activiryCheckinList")
                                el-table-column(label="姓名" prop="username")
                                el-table-column(label="性别" prop="gender")
                                el-table-column(label="电话" prop="phone")
                                el-table-column(label="QQ" prop="qq")
                                el-table-column(label="Wechat" prop="wechat")
                                el-table-column(label="签到时间" prop="checkin_time")
</template>
<script>
import axios from 'axios'
export default {
  components: {},
  data() {
    return {
      clubList: [],
      selectClub: '',
      activityList: [],
      selectActivity: [],
      activityApplyListData: [],
      activiryCheckinList: [],
      updateActivityForm: {
        name: '',
        imgUrl: '',
        intro: '',
        max_people: '',
        enable_checkin: '',
        start_time: '',
        end_time: '',
        type: '',
        status: ''
      },
      whichIsShow: 'list',
      rules: {
        name: [{ required: true, message: '请输入活动名称', trigger: 'blur' }],
        imgUrl: [
          { required: true, message: '请输入海报地址', trigger: 'blur' }
        ],
        intro: [{ required: true, message: '请输入活动介绍', trigger: 'blur' }],
        enable_checkin: [
          { required: true, message: '请选择活动是否签到', trigger: 'blur' }
        ],
        start_time: [
          { required: true, message: '请选择活动开始时间', trigger: 'blur' }
        ],
        end_time: [
          { required: true, message: '请选择活动结束时间', trigger: 'blur' }
        ],
        type: [{ required: true, message: '请选择活动类型', trigger: 'blur' }],
        status: [{ required: true, message: '请选择活动状态', trigger: 'blur' }]
      }
    }
  },
  mounted() {
    this.fillData()
  },
  methods: {
    async fillData() {
      let { data } = await axios.get('/api/club/getmyadminclublist')
      if (data.getMyClubListResultCode == '0') {
        //console.log('LOGIN!')
      }
      if (data.queryResult == '1') {
        this.clubList = data.queryData
      }
    },
    async getActivityData() {
      let { data } = await axios.get(
        '/api/activity/mymanageractivity?clubId=' + this.selectClub
      )
      this.activityList = data
    },
    async showActivityDetailPanel(val) {
      let { data } = await axios.get(
        '/api/activity/activityapplylist?activityId=' + val.id
      )
      this.activityApplyListData = data
      this.selectActivity = val
      this.getCheckinList()
      //this.updateActivityForm.clubId = val.id
      this.updateActivityForm.name = val.name
      this.updateActivityForm.imgUrl = val.imgUrl
      this.updateActivityForm.intro = val.intro
      this.updateActivityForm.max_people = val.max_people
      this.updateActivityForm.type = val.type.toString()
      this.updateActivityForm.enable_checkin = val.enable_checkin.toString()
      this.updateActivityForm.start_time = val.start_time
      this.updateActivityForm.end_time = val.end_time
      this.updateActivityForm.status = val.status.toString()
      this.whichIsShow = 'clubPanel'
    },
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.updateActivityInfo()
        } else {
          this.$notify.error({
            title: '填写不完整',
            message: '请将表单填写完整'
          })
        }
      })
    },
    async updateActivityInfo() {
      let { data } = await axios.post(
        `/api/activity/editactivity?activityId=` + this.selectActivity.id,
        {
          activityName: this.updateActivityForm.name,
          imgUrl: this.updateActivityForm.imgUrl,
          intro: this.updateActivityForm.intro,
          maxPeople: this.updateActivityForm.max_people,
          activityType: this.updateActivityForm.type,
          status: this.updateActivityForm.status,
          enable_checkin: this.updateActivityForm.enable_checkin,
          startTime: this.updateActivityForm.start_time,
          endTime: this.updateActivityForm.end_time
        }
      )
      if (data.editActivityResult === '1') {
        this.$notify({
          title: '成功更新活动',
          message: '操作成功啦',
          type: 'success'
        })
      } else {
        this.$notify.error({
          title: '操作失败',
          message: '系统错误信息：' + data.errMsg
        })
      }
    },
    async deteleActivity() {
      let { data } = await axios.get(
        '/api/activity/deleteActivity?activityId=' + this.selectActivity.id
      )
      if (data.deleteActivityResult === '1') {
        this.$notify({
          title: '成功删除活动',
          message: '操作成功啦',
          type: 'success'
        })
      } else {
        this.$notify.error({
          title: '操作失败',
          message: '系统错误信息：' + data.errMsg
        })
      }
    },
    async getCheckinList() {
      let { data } = await axios.get(
        '/api/activity/activityCheckinList?activityId=' + this.selectActivity.id
      )
      this.activiryCheckinList = data
    }
  }
}
</script>
<style lang="scss" scoped>
.clubSelect {
  margin: 20px 0px;
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
