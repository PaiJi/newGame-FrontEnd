<template lang="pug">
    el-form(:model="addClubForm" label-width="100px").container
        el-row.form-title
            el-col
                h2 新增社团
                hr
        el-row
            el-col(:span="12")
                el-form-item(label="社团名称" prop="name")
                    el-input(v-model="addClubForm.clubName")
        el-row
            el-col(:span="12")
                el-form-item(label="社团简介" prop="intro")
                    el-input(type="textarea" :row=4 placeholder="随便写点什么，让大家了解你的社团" v-model="addClubForm.intro")
        el-row
            el-col(:span="12")
                el-form-item(label="社团归属" prop="clubBelong")
                    el-select(placeholder="请选择社团归属" v-model="addClubForm.clubBelong")
                        el-option(label="电子信息工程" value="1")
                        el-option(label="团委" value="2")
        el-row
            el-col(:span="12")
                el-form-item(label="社团负责人" prop="clubadmin")
                    el-input(v-model="addClubForm.clubAdmin" placeholder="输入该用户的UID")
        el-row
            el-col
                el-form-item(label="加入方式" prop="joinmode")
                    el-radio-group(v-model="addClubForm.joinMode")
                        el-radio(label="manual") 人工审核
                        el-radio(label="invite") 仅邀请
                        el-radio(label="unlimit") 无限制
                        el-radio(label="unallow") 不允许新成员
        el-row
            el-col
                el-form-item(label="社团状态" prop="status")
                    el-radio-group(v-model="addClubForm.status")
                        el-radio(label="ready") 筹备中
                        el-radio(label="running") 正常运营
                        el-radio(label="warning") 运营异常
                        el-radio(label="closed") 已注销
        el-row
            el-col
                el-form-item
                    el-button(type="primary" ) 立即创建
</template>
<style lang="scss">
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
      addClubForm: {
        clubName: '',
        intro: '',
        clubBelong: '',
        clubAdmin: '',
        joinMode: '',
        status: ''
      }
    }
  },
  methods: {
    async add() {
      let clubName = this.addClubForm.clubName
      let intro = this.addClubForm.intro
      let clubBelong = this.addClubForm.clubBelong
      let clubAdmin = this.addClubForm.clubAdmin
      let joinMode = this.addClubForm.joinMode
      let status = this.addClubForm.status
      let { data } = await axios.post(
        `http://localhost:2333/newGame-BackEnd/public/index.php/api/club/add`,
        {
          clubName: clubName,
          intro: intro,
          clubBelong: clubBelong,
          clubAdmin: clubAdmin,
          joinMode: joinMode,
          status: status
        }
      )
      if (data.statusCode === '200') {
        console.log('success')
      }
    }
  }
}
</script>
