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
      .clubPanel(v-if="whichIsShow=='clubPanel'")
        el-tabs(type="border-card")
          el-tab-pane(label="修改社团信息").clubDetail
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
          el-tab-pane(label="修改纳新问卷")
            editQuestion(v-bind:selectClub="selectClub.id")
          el-tab-pane(label="审批管理") 审批管理


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
      whichIsShow: 'list'
    }
  },
  mounted() {
    this.fillData()
  },
  methods: {
    async fillData() {
      console.log('Start GET Data')
      let { data } = await axios.get('/api/club/getmyclublist')
      console.log(data)
      if (data.getMyClubListResultCode == '0') {
        console.log('LOGIN!')
      }
      if (data.queryResult == '1') {
        this.clubList = data.queryData
      }
    },
    async showClubDetailPanel(val) {
      console.log(val)
      this.selectClub = val
      this.whichIsShow = 'clubPanel'
      let { data } = await axios.get(
        '/api/club/getclubcontact?clubid=' + this.selectClub.id
      )
      this.clubContact = data
    }
  }
}
</script>
<style lang="scss">
.clubDetail {
  .el-row {
    border-bottom: solid 1px rgba(240, 240, 240, 1);
  }
  .el-col-md-4 {
    color: gray;
  }
}
</style>
