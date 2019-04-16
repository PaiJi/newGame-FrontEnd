<template lang="pug">
    .container
        el-table(:data='clubList', style='width: 100%' v-loading="loading")
            el-table-column(type='expand')
                template(slot-scope='scope')
                    el-form.demo-table-expand(label-position='left', inline='')
                        el-form-item(label='社团名称')
                            span {{ scope.row.name }}
                        el-form-item(label='所属部门')
                            span {{ scope.row.belong }}
                        el-form-item(label='社团 ID')
                            span {{ scope.row.id }}
                        el-form-item(label='创建时间')
                            span {{ scope.row.create_time }}
                        el-form-item(label='社团分类')
                            span {{ scope.row.sort }}
                        el-form-item(label='加入模式')
                            span {{ scope.row.join_mode }}
                        el-form-item(label='社团描述')
                            span {{ scope.row.intro }}
            el-table-column(label='申请日期', width='180')
                template(slot-scope='scope')
                    i.el-icon-time
                    span(style='margin-left: 10px') {{ scope.row.create_time }}
            el-table-column(label='名称', width='180')
                template(slot-scope='scope')
                    .name-wrapper(slot='reference')
                        el-tag(size='medium') {{ scope.row.name }}
            el-table-column(label='操作')
                template(slot-scope='scope')
                    el-button(size='mini', type='success',@click='handleEdit(scope.$index, scope.row)') 通过
                    el-button(size='mini', type='danger', @click='handleDelete(scope.$index, scope.row)') 拒绝


</template>
<style lang="scss">
.demo-table-expand {
  font-size: 0;
}
.demo-table-expand label {
  width: 90px;
  color: #99a9bf;
}
.demo-table-expand .el-form-item {
  margin-right: 0;
  margin-bottom: 0;
  width: 100%;
}
</style>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      clubList: [],
      loading: true
    }
  },
  mounted() {
    this.fetchClubData()
  },
  methods: {
    async fetchClubData() {
      let { data } = await axios.get('/api/club/clublist?option=status&value=2')
      console.log(data)
      this.clubList = data
      this.loading = false
    },
    handleEdit(index, row) {
      console.log(index, row)
    },
    handleDelete(index, row) {
      console.log(index, row)
    }
  }
}
</script>
