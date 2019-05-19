<template lang="pug">
    el-row(:gutter='20' )
        el-col(:span='10')
            .grid-content
                el-form(:model="ruleForm" :rules="rules" :hide-required-asterisk="true" ref="ruleForm" label-width="70px")
                    el-form-item(label="邮箱" prop="email")
                        el-input(v-model="ruleForm.email")
                    el-form-item(label="密码" prop="password")
                        el-input(v-model="ruleForm.password" type="password")
                    el-form-item
                        el-button(type="primary" style="width:100%" @click="submitForm('ruleForm')") 修改
</template>
<script>
import axios from 'axios'
export default {
  data() {
    return {
      ruleForm: {
        email: '',
        password: ''
      },
      rules: {
        email: [
          { required: true, message: '请输入登录邮箱', trigger: 'blur' },
          {
            type: 'email',
            required: true,
            message: '邮箱格式不正确',
            trigger: 'change'
          },
          { max: 255, message: '长度不得多于255个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          {
            min: 6,
            message: '太短啦！再长点'
          },
          { max: 30, message: '密码太长了！' }
        ]
      }
    }
  },
  mounted() {
    this.getUserInfo()
  },
  methods: {
    async getUserInfo() {
      let { data } = await axios.get('/api/user/getuserinfo')
      if (data.loginStatus == 'true') {
        this.ruleForm.email = data.userInfo.email
      }
    },
    async updateInfo() {
      let { data } = await axios.post(`/api/user/updateUserAccountSafeInfo`, {
        email: this.ruleForm.email,
        password: this.ruleForm.password
      })
      if (data.updateInfoResultCode === '0') {
        this.$notify.error({
          title: '更新失败',
          message: data.errMsg
        })
      }
      if (data.updateInfoResultCode === '1') {
        this.$notify({
          title: '信息修改成功',
          message: '恭喜，您的信息已经修改成功啦。',
          type: 'success'
        })
      }
    },
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.updateInfo()
        } else {
          this.$notify.error({
            title: '表单不完整',
            message: '请填写所有表单内容'
          })
        }
      })
    },
    resetForm(formName) {
      this.$refs[formName].resetFields()
    }
  }
}
</script>
