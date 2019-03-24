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
                                template(slot='title') 社团
                                el-menu-item(index='1-1' @click="currtentMainView='addClub'") 新增
                                el-menu-item(index='1-2') 审批
                                el-menu-item(index='1-3') 管理
                            //el-menu-item-group(title='分组2')
                                el-menu-item(index='1-3') 表单列表
                            //el-submenu(index='1-4')
                                template(slot='title') 选项4
                                el-menu-item(index='1-4-1') 选项1
                        el-menu-item(index='2' @click="currtentMainView='clubList'")
                            i.el-icon-menu
                            span(slot='title') 社团列表
                        el-menu-item(index='3', disabled='')
                            i.el-icon-document
                            span(slot='title') 活动列表
                        el-menu-item(index='4' @click="currtentMainView='setting'")
                            i.el-icon-setting
                            span(slot='title') 系统设置
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
                                //el-submenu(index='3-4')
                                    template(slot='title') 选项4
                                    el-menu-item(index='3-4-1') 选项1
                                    el-menu-item(index='3-4-2') 选项2
                                    el-menu-item(index='3-4-3') 选项3

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
import addClub from '~/components/home/club/addClub'
import clubList from '~/components/home/clubList'
import setting from '~/components/home/setting'
import editInfo from '~/components/home/user/editInfo'
import safeSetting from '~/components/home/user/safeSetting'
export default {
  components: {
    addClub,
    index,
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
