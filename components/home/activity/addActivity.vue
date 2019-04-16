<template lang="pug">
    el-form(:model="addClubForm" label-width="100px").container
        el-row.form-title
            el-col
                h2 创建新活动
                hr
        el-row
            el-col(:md=20)
                el-form-item(label="活动名称" prop="name")
                    el-input(v-model="addClubForm.name")
        el-row
            el-col(:md=20)
                el-form-item(label="海报地址" prop="imgUrl")
                    el-input(v-model="addClubForm.imgUrl")
        el-row
            el-col(:md=20)
                el-form-item(label="活动简介" prop="intro")
                    el-input(type="textarea" :row=4 placeholder="随便写点什么，让大家了解你的社团活动" v-model="addClubForm.intro")
        el-row
            el-col(:md=20)
                el-form-item(label="最大人数" prop="max_people")
                    el-input(v-model="addClubForm.max_people" placeholder="无限制直接留空")
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
        name: '',
        imgUrl: '',
        intro: '',
        max_people: '',
        type: '',
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
