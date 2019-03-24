<template lang="pug">
    el-row
        el-col(:span='18').header-left
            <img src="data:image/png;base64,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">
            .intro
                h3 NewGame
                h1 可能是东半球第二好用的社团管理工具
        el-col(:span='6' style="text-align:right")
            .button-group
                el-button 注册
                el-button 登录
</template>
<style lang="scss">
.el-header {
  background-color: #fafafa;
  .header-left {
    display: flex;
    align-items: center;
  }
  .intro {
    h1,
    h3 {
      padding: 3px 0px;
      color: #111;
      font-family: 'SF Pro SC', 'SF Pro Text', 'SF Pro Icons', 'PingFang SC',
        'Helvetica Neue', 'Helvetica', 'Arial', sans-serif;
    }
  }
  .el-row {
    display: flex;
    align-items: center;
  }
  .button-group {
    margin: 0 auto;
  }
}
</style>
