<template lang="pug">
    el-container
        el-aside
            el-row.tac
                el-col(:span='24' style="text-align:center;padding:20px;")
                    <img src="data:image/png;base64,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">
                    h4 {{userInfo.nickname}}
                    p {{userInfo.email}}
                    p {{userInfo.admin}}
            el-row
                el-col
                    el-menu.el-menu-vertical-demo(default-active='', @open='handleOpen', @close='handleClose')
                        el-submenu(index='1')
                            template(slot='title')
                                i.el-icon-location
                                span 社团管理
                            el-menu-item-group
                                el-menu-item(index='1-1' @click="currtentMainView='myClubList'") 社团列表
                                el-menu-item(index='1-2' @click="currtentMainView='managerClub'") 社团管理
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
                                    el-menu-item(index='2-1' @click="currtentMainView='myActivityList'") 活动列表
                                    el-menu-item(index='2-2' @click="currtentMainView='managerActivity'") 活动管理
                                    el-menu-item(index='2-3' @click="currtentMainView='addActivity'") 申请新活动
                        el-menu-item(index='4' @click="currtentMainView='setting'")
                            i.el-icon-setting
                            span(slot='title') 设置
        el-container
            el-header
                el-row
                    el-col
                        el-menu.el-menu-demo(:default-active='activeIndex', mode='horizontal', @select='handleSelect')
                            el-menu-item(index='1' @click="currtentMainView='index'") 个人中心
                            //el-menu-item(index='2') 消息中心
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
  background-color: rgba(211, 220, 230, 0.58);
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
import axios from 'axios'
//导入axios
import index from '~/components/home/index'
//主界面导入分割线
import addClub from '~/components/home/club/addClub'
import myClubList from '~/components/home/club/myClubList'
import clubList from '~/components/home/clubList'
import managerClub from '~/components/home/club/managerClub'
//社团管理组件导入分割线
import myActivityList from '~/components/home/activity/myActivity'
import addActivity from '~/components/home/activity/addActivity'
import managerActivity from '~/components/home/activity/managerActivity'
//活动管理组件导入分割线
import setting from '~/components/home/setting'
import editInfo from '~/components/home/user/editInfo'
import safeSetting from '~/components/home/user/safeSetting'
export default {
  components: {
    index,
    clubList,
    myClubList,
    managerClub,
    addClub,
    myActivityList,
    addActivity,
    managerActivity,
    setting,
    editInfo,
    safeSetting
  },
  data() {
    return {
      activeIndex: '1',
      activeIndex2: '1',
      currtentMainView: 'index',
      userInfo: []
    }
  },
  mounted() {
    this.getUserInfo()
  },
  methods: {
    async getUserInfo() {
      let { data } = await axios.get('/api/user/getuserinfo')
      if (data.loginStatus == 'true') {
        this.userInfo = data.userInfo
      }
    },
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
