<template lang="pug">
    .container
        el-row
            el-col
                h2 系统设置
                hr
        el-collapse.collapse-padding
            el-collapse-item(title="系统控制")
                el-row
                    el-col
                        el-switch(v-model="systemPowerSwitch" active-text="系统开放" inactive-text="系统关闭")
            el-collapse-item(title="超级管理员控制" )
                el-row(:gutter='20')
                    el-col(:span='4')
                        el-input(v-model="userid" placeholder="输入用户ID")
                    el-col(:span='4')
                        el-button(type="danger") 添加超级管理员
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
export default {
  data() {
    return {
      systemPowerSwitch: false,
      userid: ''
    }
  }
}
</script>
