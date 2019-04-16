<template lang="pug">
    el-container
        el-aside
            el-row.tac
                el-col(:span='24')
                    img(src="")
                    i.el-icon-loading
                    p ADMIN
                    el-menu.el-menu-vertical-demo(default-active='', @open='handleOpen', @close='handleClose')
                        el-submenu(index='1')
                            template(slot='title')
                                i.el-icon-location
                                span 社团管理
                            el-menu-item-group
                                template(slot='title') 我的社团
                                el-menu-item(index='1-1' @click="currtentMainView='myClubList'") 社团列表
                                el-menu-item(index='1-2') 我的审批
                                el-menu-item(index='1-3' @click="currtentMainView='addClub'") 申请新社团
                            //el-menu-item-group(title='分组2')
                                el-menu-item(index='1-3') 表单列表
                            //el-submenu(index='1-4')
                                template(slot='title') 选项4
                                el-menu-item(index='1-4-1') 选项1
                        el-menu
                            el-submenu(index='2')
                                template(slot='title')
                                    i.el-icon-document
                                    span 活动管理
                                el-menu-item-group
                                    el-menu-item(index='2-1') 活动列表
                                    el-menu-item(index='2-2') 活动管理
                                    el-menu-item(index='2-3' @click="currtentMainView='addActivity'") 申请新活动
                        el-menu-item(index='4' @click="currtentMainView='setting'")
                            i.el-icon-setting
                            span(slot='title') 设置
        el-container
            el-header
                el-row
                    el-col(:span="24" )
                        el-menu.el-menu-demo(:default-active='activeIndex', mode='horizontal', @select='handleSelect')
                            el-menu-item(index='1') 处理中心
                            el-menu-item(index='2') 消息中心
                            el-submenu(index='3')
                                template(slot='title') 用户管理
                                el-menu-item(index='3-1' @click="currtentMainView='editInfo'") 信息修改
                                el-menu-item(index='3-2' @click="currtentMainView='safeSetting'") 安全设置
                                el-menu-item(index='3-3') 登出

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
//主界面导入分割线
import addClub from '~/components/home/club/addClub'
import myClubList from '~/components/home/club/myClubList'
import clubList from '~/components/home/clubList'
//社团管理组件导入分割线
import addActivity from '~/components/home/activity/addActivity'
//活动管理组件导入分割线
import setting from '~/components/home/setting'
import editInfo from '~/components/home/user/editInfo'
import safeSetting from '~/components/home/user/safeSetting'
export default {
  components: {
    index,
    clubList,
    myClubList,
    addClub,
    addActivity,
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
  methods: {
    handleSelect(key, keyPath) {
      console.log(key, keyPath)
    },
    handleOpen(key, keyPath) {
      console.log(key, keyPath)
    },
    handleClose(key, keyPath) {
      console.log(key, keyPath)
    }
  }
}
</script>
