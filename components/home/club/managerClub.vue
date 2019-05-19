<template lang="pug">
    .container
        .clubList(v-if="whichIsShow=='list'")
            el-row
                el-col
                    el-table(:data="clubList" @current-change="showClubDetailPanel")
                        el-table-column(label="社团名称" prop="name" width="")
                        el-table-column(label="社团简介" prop="intro" width="")
                        el-table-column(label="社团人数" prop="members" width="")
                        el-table-column(label="注册时间" prop="create_time" width="")
        .clubDetailContainer(v-if="whichIsShow=='clubPanel'")
            el-row.clubStatusBar
                el-col(:span='4')
                    el-button(@click="whichIsShow='list'") 返回列表
                el-col(:span='20')
                    h4 {{selectClub.name}}
                    span 详细信息
            .clubPanel
                el-tabs(type="border-card")
                    el-tab-pane(label="修改社团信息").clubDetail
                        el-form(:model="updateClubForm" label-width="100px")
                            el-row
                                el-col(:span="12")
                                    el-form-item(label="社团名称" prop="name")
                                        el-input(v-model="updateClubForm.clubName")
                            el-row
                                el-col(:span="12")
                                    el-form-item(label="LOGO地址" prop="imgUrl")
                                        el-input(v-model="updateClubForm.imgUrl")
                            el-row
                                el-col(:span="12")
                                    el-form-item(label="社团简介" prop="intro")
                                        el-input(type="textarea" :row=8 placeholder="随便写点什么，让大家了解你的社团" v-model="updateClubForm.intro")
                            el-row
                                el-col(:span="12")
                                    el-form-item(label="社团归属" prop="clubBelong")
                                        el-select(placeholder="请选择社团归属" v-model="updateClubForm.clubBelong")
                                            el-option(label="电子信息工程" value="2")
                                            el-option(label="团委" value="1")
                            el-row
                                el-col(:span="12")
                                    el-form-item(label="社团分类" prop="clubSort")
                                        el-select(placeholder="请选择社团类别" v-model="updateClubForm.clubSort")
                                            el-option(label="技术类" value="1")
                                            el-option(label="娱乐类" value="2")
                            el-row
                                el-col(:span="12")
                                    el-form-item(label="社团负责人" prop="clubadmin")
                                        el-input(v-model="updateClubForm.clubAdmin" placeholder="输入该用户的UID")
                            el-row
                                el-col
                                    el-form-item(label="加入方式" prop="joinmode")
                                        el-radio-group(v-model="updateClubForm.joinMode")
                                            el-radio(label="1") 人工审核
                                            //el-radio(label="2" disabled) 仅邀请
                                            el-radio(label="3") 无限制
                                            el-radio(label="0") 不允许新成员
                            el-row
                                el-col
                                    el-form-item(label="社团状态" prop="status")
                                        el-radio-group(v-model="updateClubForm.status")
                                            el-radio(label="2") 筹备中
                                            el-radio(label="1") 正常运营
                                            el-radio(label="3") 运营异常
                                            el-radio(label="0") 已注销
                            el-row
                                el-col
                                    el-form-item
                                        el-button(type="primary" @click="updateClubInfo") 更新信息
                    el-tab-pane(label="修改纳新问卷")
                        editQuestion(v-bind:selectClub="selectClub.id")
                    el-tab-pane(label="审批管理")
                        el-table(:data="applyList" @current-change="showApplyModal")
                            el-table-column(label="姓名" prop="userInfo.username")
                            el-table-column(label="性别" prop="userInfo.gender")
                            el-table-column(label="电话" prop="userInfo.phone")
                            el-table-column(label="QQ" prop="userInfo.qq")
                            el-table-column(label="WECHAT" prop="userInfo.wechat")
                            el-table-column(label="学院" prop="userInfo.department")
                            el-table-column(label="专业" prop="userInfo.major")
                        el-dialog(title="表单详情" :visible.sync="applyContentDialogVisible" width="80%")
                            el-form(label-position="top")
                                el-form-item(v-for="(item,index) in applyContent" :label="index+'.'+item.question")
                                    p 答案：{{item.answer}}
                            span(slot="footer")
                                el-button(type="success" @click="handleApply(true)" :disabled="selectApply.status!=`待审核`") 通过
                                el-button(type="danger" @click="handleApply(false)" :disabled="selectApply.status!=`待审核`") 拒绝

</template>
<script>
import axios from 'axios'
import editQuestion from '~/components/home/club/editQuestion'
export default {
  components: {
    editQuestion
  },
  data() {
    return {
      clubList: [],
      selectClub: [],
      updateClubForm: {
        clubId: '',
        clubName: '',
        imgUrl: '',
        intro: '',
        clubBelong: '',
        clubSort: '',
        clubAdmin: '',
        joinMode: '',
        status: ''
      },
      applyList: [],
      selectApply: [],
      applyContent: [],
      applyContentDialogVisible: false,
      whichIsShow: 'list'
    }
  },
  mounted() {
    this.fillData()
  },
  methods: {
    async fillData() {
      let { data } = await axios.get('/api/club/getMyAdminClubList')
      if (data.getMyClubListResultCode == '0') {
        console.log('LOGIN!')
      }
      if (data.queryResult == '1') {
        this.clubList = data.queryData
      }
    },
    async getContact() {
      let { data } = await axios.get(
        '/api/club/getclubcontact?clubid=' + this.selectClub.id
      )
      this.clubContact = data
    },
    async getApplyList() {
      let { data } = await axios.get(
        '/api/club/getApplyList?clubId=' + this.selectClub.id
      )
      this.applyList = data
    },
    showClubDetailPanel(val) {
      //console.log(val)
      this.selectClub = val
      this.updateClubForm.clubId = val.id
      this.updateClubForm.clubName = val.name
      this.updateClubForm.imgUrl = val.img_logo
      this.updateClubForm.intro = val.intro
      this.updateClubForm.clubBelong = val.belong.toString()
      this.updateClubForm.clubSort = val.sort.toString()
      this.updateClubForm.clubAdmin = val.user_id
      this.updateClubForm.joinMode = val.join_mode.toString()
      this.updateClubForm.status = val.status.toString()
      this.getApplyList()
      this.getContact()
      this.whichIsShow = 'clubPanel'
    },
    async updateClubInfo() {
      let { data } = await axios.post(`/api/club/updateclub`, {
        clubId: this.updateClubForm.clubId,
        clubName: this.updateClubForm.clubName,
        imgUrl: this.updateClubForm.imgUrl,
        intro: this.updateClubForm.intro,
        clubBelong: this.updateClubForm.clubBelong,
        clubSort: this.updateClubForm.clubSort,
        clubAdmin: this.updateClubForm.clubAdmin,
        joinMode: this.updateClubForm.joinMode,
        status: this.updateClubForm.status
      })
      if (data.updateClubResult === '1') {
        this.$notify({
          title: '成功更新社团',
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
    async showApplyModal(val) {
      this.selectApply = val
      let { data } = await axios.get(
        '/api/club/applyContent?clubId=' +
          this.selectClub.id +
          '&applyId=' +
          val.id
      )
      this.applyContent = data
      for (const item of this.applyContent) {
        if (item.type == 'checkbox') {
          item.answer = JSON.parse(item.answer)
        }
      }
      this.applyContentDialogVisible = true
    },
    async handleApply(val) {
      console.log(val)
      let { data } = await axios.post(
        '/api/club/handleApply?applyId=' + this.applyContent[0].apply_id,
        {
          handleContent: val
        }
      )
      if (data.handleApplyResultCode == '1') {
        this.$notify({
          title: '成功审批',
          message: '操作成功啦',
          type: 'success'
        })
        this.applyContentDialogVisible = false
      } else {
        this.$notify.error({
          title: '操作失败',
          message: '系统错误信息：' + data.errMsg
        })
        this.applyContentDialogVisible = false
      }
    }
  }
}
</script>
<style lang="scss" scoped>
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
