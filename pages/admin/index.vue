<template lang="pug">
    el-container
        el-aside
            el-row.tac
                el-col(:span='24')
                    el-menu.el-menu-vertical-demo(default-active='')
                        el-submenu(index='1')
                            template(slot='title')
                                i.el-icon-location
                                span 社团管理
                            el-menu-item-group
                                template(slot='title') 社团
                                el-menu-item(index='1-1' @click="currtentMainView='addClub'") 新增
                                el-menu-item(index='1-2' @click="currtentMainView='review'") 审批
                                el-menu-item(index='1-3') 管理
                            //el-menu-item-group(title='分组2')
                                el-menu-item(index='1-3') 表单列表
                            //el-submenu(index='1-4')
                                template(slot='title') 选项4
                                el-menu-item(index='1-4-1') 选项1
                        el-menu-item(index='2' @click="currtentMainView='clubList'")
                            i.el-icon-menu
                            span(slot='title') 社团列表
                        el-menu-item(index='3')
                            i.el-icon-document
                            span(slot='title') 活动列表
                        el-menu-item(index='4' @click="currtentMainView='setting'")
                            i.el-icon-setting
                            span(slot='title') 系统设置
        el-container
            el-header
                el-row
                    el-col(:span="24" style="text-align:center;background-color:white;")
                        h3 newGame控制面板

            el-main
                component(:is="currtentMainView")
</template>
<style lang="scss">
.el-header,
.el-footer {
  background-color: #b3c0d1;
  color: #333;
  //text-align: center;
  line-height: 60px;
}
.el-header {
  padding: 0px;
}
.el-aside {
  background-color: #d3dce6;
  color: #333;
  //text-align: center;
  //line-height: 0px;
  height: 100vh;
}
.el-main {
  background-color: #e9eef378;
  color: #333;
  //text-align: center;
  //line-height: 160px;
}
</style>
<script>
import index from '~/components/home/index'
import addClub from '~/components/admin/club/addClub'
import review from '~/components/admin/club/review'
import clubList from '~/components/home/clubList'
import setting from '~/components/home/setting'
import editInfo from '~/components/home/user/editInfo'
import safeSetting from '~/components/home/user/safeSetting'
export default {
  components: {
    index,
    addClub,
    review,
    clubList,
    setting,
    editInfo,
    safeSetting
  },
  data() {
    return {
      activeIndex: '1',
      activeIndex2: '1',
      currtentMainView: 'index'
    }
  },
  methods: {}
}
</script>
