<template lang="pug">
    .container
        el-table(:data='myActivityList', style='width: 100%')
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
                    el-button(size='mini', type='danger', @click='handleDelete(scope.row.activity_id)') 退出

</template>
<style lang="scss">
</style>
<script>
import axios from 'axios'
export default {
  data() {
    return {
      myActivityList: []
    }
  },
  mounted() {
    this.getMyActivityList()
  },
  methods: {
    handleEdit(index, row) {
      console.log(index, row)
    },
    async handleDelete(activity_id) {
      console.log(activity_id)
      let { data } = await axios.get(
        '/api/activity/exitactivity?activityId=' + activity_id
      )
      if (data.queryResult === '1') {
        this.$notify({
          title: '操作成功',
          message: '已退出该活动',
          type: 'success'
        })
      }
    },
    async getMyActivityList() {
      let { data } = await axios.get('/api/activity/getmyactivity')
      this.myActivityList = data.queryData
    }
  }
}
</script>
