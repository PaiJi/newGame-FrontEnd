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
                el-form-item(label="LOGO地址" prop="imgUrl")
                    el-input(v-model="addClubForm.imgUrl")
        el-row
            el-col(:span="12")
                el-form-item(label="社团简介" prop="intro")
                    el-input(type="textarea" :row=4 placeholder="随便写点什么，让大家了解你的社团" v-model="addClubForm.intro")
        el-row
            el-col(:span="12")
                el-form-item(label="社团归属" prop="clubBelong")
                    el-select(placeholder="请选择社团归属" v-model="addClubForm.clubBelong")
                        el-option(label="电子信息工程" value="2")
                        el-option(label="团委" value="1")
        el-row
            el-col(:span="12")
                el-form-item(label="社团分类" prop="clubSort")
                    el-select(placeholder="请选择社团类别" v-model="addClubForm.clubSort")
                        el-option(label="技术类" value="1")
                        el-option(label="娱乐类" value="2")
        el-row
            el-col(:span="12")
                el-form-item(label="社团负责人" prop="clubadmin")
                    el-input(v-model="addClubForm.clubAdmin" placeholder="输入该用户的UID")
        el-row
            el-col
                el-form-item(label="加入方式" prop="joinmode")
                    el-radio-group(v-model="addClubForm.joinMode")
                        el-radio(label="1") 人工审核
                        el-radio(label="2") 仅邀请
                        el-radio(label="3") 无限制
                        el-radio(label="0") 不允许新成员
        el-row
            el-col
                el-form-item(label="社团状态" prop="status")
                    el-radio-group(v-model="addClubForm.status")
                        el-radio(label="2") 筹备中
                        el-radio(label="1") 正常运营
                        el-radio(label="3") 运营异常
                        el-radio(label="0") 已注销
        el-row
            el-col
                el-form-item
                    el-button(type="primary" @click="add") 立即创建
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
        imgUrl: '',
        intro: '',
        clubBelong: '',
        clubSort: '',
        clubAdmin: '',
        joinMode: '',
        status: ''
      }
    }
  },
  methods: {
    async add() {
      let clubName = this.addClubForm.clubName
      let imgUrl = this.addClubForm.imgUrl
      let intro = this.addClubForm.intro
      let clubBelong = this.addClubForm.clubBelong
      let clubSort = this.addClubForm.clubSort
      let clubAdmin = this.addClubForm.clubAdmin
      let joinMode = this.addClubForm.joinMode
      let status = this.addClubForm.status
      let { data } = await axios.post(
        `/api/club/addclub`,
        {
          clubName: clubName,
          imgUrl: imgUrl,
          intro: intro,
          clubBelong: clubBelong,
          clubSort: clubSort,
          clubAdmin: clubAdmin,
          joinMode: joinMode,
          status: status
        },
        {
          withCredentials: true
        }
      )
      if (data.addClubResult === '1') {
        this.$notify({
          title: '成功添加社团',
          message: '操作成功啦，请耐心等待审核',
          type: 'success'
        })
      } else {
        this.$notify.error({
          title: '操作失败',
          message: '系统错误信息：' + data.errMsg
        })
      }
    }
  }
}
</script>
