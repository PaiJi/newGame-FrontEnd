<template lang="pug">
    .container
        el-row
            el-col
                h2 系统设置
                hr
        el-row
            el-col
                p 开始操作前，请确保您知道自己在做什么，大部分情况下，无需对社团和活动人数进行校正。
        el-collapse.collapse-padding
            el-collapse-item(title="系统控制")
                el-row
                    el-col(:span='6')
                        el-switch(v-model="systemPowerSwitch" active-text="系统开放" inactive-text="系统关闭")
                    el-col(:span='4')
                        el-button(type="danger" @click="systemSwitch") 确认
            el-collapse-item(title="超级管理员控制" )
                el-row(:gutter='20')
                    el-col(:span='4')
                        el-input(v-model="userId" placeholder="输入用户ID")
                    el-col(:span='4')
                        el-button(type="danger" @click="addAdmin") 添加超级管理员
            el-collapse-item(title="指定活动参与统计人数校正" )
                el-row(:gutter='20')
                    el-col(:span='4')
                        el-input(v-model="activityId" placeholder="输入活动ID")
                    el-col(:span='4')
                        el-button(type="danger" @click="regulateActivityPeople") 开始校正
            el-collapse-item(title="指定社团成员人员校正" )
                el-row(:gutter='20')
                    el-col(:span='4')
                        el-input(v-model="clubId" placeholder="输入社团ID")
                    el-col(:span='4')
                        el-button(type="danger" @click="regulateClubPeople") 开始校正
</template>
<style lang="scss">
.collapse-padding {
  .el-collapse-item__header {
    padding: 0px 10px;
  }
  .el-row {
    padding: 0px 10px;
  }
}
hr {
  border-color: rgb(244, 244, 244);
  border-width: 1px;
  margin-top: 10px;
  margin-bottom: 20px;
}
</style>


<script>
import axios from 'axios'
export default {
  data() {
    return {
      systemPowerSwitch: true,
      userId: '',
      activityId: '',
      clubId: ''
    }
  },
  methods: {
    async systemSwitch() {
      let { data } = await axios.get(
        '/api/index/systemPowerSwitch?handleValue=' + this.systemPowerSwitch
      )
      if (data.switchResult == '1') {
        this.$notify({
          title: '操作成功',
          message: '已修改系统运行状态',
          type: 'success'
        })
      } else {
        this.$notify.info({
          title: '越权操作',
          message: '您并非管理员，无权进行此操作。'
        })
      }
    },
    async addAdmin() {
      let { data } = await axios.get('/api/user/opuser?userId=' + this.userId)
      if (data.opResult == '1') {
        this.$notify({
          title: '授权成功',
          message: '该用户已是管理员权限',
          type: 'success'
        })
      } else {
        this.$notify.info({
          title: '操作失败',
          message: data.errMsg
        })
      }
    },
    async regulateActivityPeople() {
      let { data } = await axios.get(
        '/api/activity/regulatePeople?activityId=' + this.activityId
      )
      if (data.queryResult == '1') {
        this.$notify({
          title: '校正成功',
          message: '活动参与人数已刷新',
          type: 'success'
        })
      } else {
        this.$notify.info({
          title: '无需校正',
          message: '该活动统计人数正常'
        })
      }
    },
    async regulateClubPeople() {
      let { data } = await axios.get(
        '/api/club/regulatePeople?clubId=' + this.clubId
      )
      if (data.queryResult == '1') {
        this.$notify({
          title: '校正成功',
          message: '社团加入人数已刷新',
          type: 'success'
        })
      } else {
        this.$notify.info({
          title: '无需校正',
          message: '该社团统计人数正常'
        })
      }
    }
  }
}
</script>
