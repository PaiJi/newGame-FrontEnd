<template lang="pug">
    el-form(:model="addClubForm" :rules="rules" ref="ruleForm" label-width="150px").container
        el-row.form-title
            el-col
                h2 创建新活动
                hr
        el-row
            el-col
                el-form-item(label="选择管理社团" prop="selectClub")
                    el-select(v-model="selectClub" placeholder="选择要添加活动的社团")
                        el-option(v-for="item in clubslist" :key="item.id" :label="item.name" :value="item.id")
        .hide-area(v-if="selectClub!=''")
            el-row
                el-col(:md='20')
                    el-form-item(label="活动名称" prop="name")
                        el-input(v-model="addClubForm.name")
            el-row
                el-col(:md='20')
                    el-form-item(label="海报地址" prop="imgUrl")
                        el-input(v-model="addClubForm.imgUrl")
            el-row
                el-col(:md='20')
                    el-form-item(label="活动简介" prop="intro")
                        el-input(type="textarea" :row=4 placeholder="随便写点什么，让大家了解你的社团活动" v-model="addClubForm.intro")
            el-row
                el-col(:md='20')
                    el-form-item(label="最大人数" prop="max_people")
                        el-input(v-model="addClubForm.max_people" placeholder="无限制直接留空")
            el-row
                el-col(:md='20')
                    el-form-item(label="活动开始时间" prop="start_time")
                        el-date-picker(v-model="addClubForm.start_time" type="datetime" value-format="yyyy-MM-dd HH:mm:ss" placeholder="请选择活动开始时间")
            el-row
                el-col(:md='20')
                    el-form-item(label="活动结束时间" prop="end_time")
                        el-date-picker(v-model="addClubForm.end_time" type="datetime" value-format="yyyy-MM-dd HH:mm:ss" placeholder="请选择活动结束时间")
            el-row
                el-col
                    el-form-item(label="活动类型" prop="type")
                        el-radio-group(v-model="addClubForm.type")
                            el-radio(label="1") 社内活动
                            el-radio(label="2") 公开活动
            el-row
                el-col
                    el-form-item(label="活动状态" prop="status")
                        el-radio-group(v-model="addClubForm.status")
                            el-radio(label="0") 未开始
                            el-radio(label="1") 报名中
                            el-radio(label="2" disabled) 已结束
            el-row
                el-col
                    el-form-item(label="签到功能" prop="enable_checkin")
                        el-radio-group(v-model="addClubForm.enable_checkin")
                            el-radio(label="0") 关闭
                            el-radio(label="1") 开启

            el-row
                el-col
                    el-form-item
                        el-button(type="primary" @click="submitForm('ruleForm')") 立即创建
</template>
<style lang="scss" scoped>
.form-title {
  margin-bottom: 40px;
  padding: 0px 30px;
}
</style>
<script>
import axios from 'axios'
export default {
  data() {
    return {
      clubslist: [],
      selectClub: '',
      addClubForm: {
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
    this.getMyAdminClubList()
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.add()
        } else {
          this.$notify.error({
            title: '填写不完整',
            message: '请将表单填写完整'
          })
        }
      })
    },
    async add() {
      let activityName = this.addClubForm.name
      let imgUrl = this.addClubForm.imgUrl
      let intro = this.addClubForm.intro
      let max_people = this.addClubForm.max_people
      let type = this.addClubForm.type
      let status = this.addClubForm.status
      let startTime = this.addClubForm.start_time
      let endTime = this.addClubForm.end_time
      let { data } = await axios.post(
        '/api/activity/addactivity?clubid=' + this.selectClub,
        {
          activityName: activityName,
          imgUrl: imgUrl,
          intro: intro,
          maxPeople: max_people,
          activityType: type,
          status: status,
          enable_checkin: this.addClubForm.enable_checkin,
          startTime: startTime,
          endTime: endTime
        }
      )
      if (data.addActivityResult === '1') {
        this.$notify({
          title: '成功添加活动',
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
    async getMyAdminClubList() {
      let { data } = await axios.get('/api/club/getmyadminclublist')
      if (data.queryResult === '1') {
        this.clubslist = data.queryData
      } else {
        this.$notify.error({
          title: '拉取管理社团列表失败',
          message: '联系管理员'
        })
      }
    }
  }
}
</script>
