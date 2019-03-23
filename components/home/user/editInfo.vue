<template lang="pug">
    el-row(:gutter='20' )
        el-col(:span='10')
            .grid-content
                el-form(:model="ruleForm" :rules="rules" :hide-required-asterisk="true" ref="ruleForm" label-width="70px")
                    el-form-item(label="姓名" prop="realname")
                        el-input(v-model="ruleForm.realname")
                    el-form-item(label="昵称" prop="nickname")
                        el-input(v-model="ruleForm.nickname")
                    el-form-item(label="年龄" prop="phone")
                        el-input(v-model="ruleForm.phone")
                    el-form-item(label="手机号" prop="phone")
                        el-input(v-model="ruleForm.phone")
                    el-form-item(label="性别" prop="gender")
                        el-radio-group(v-model="ruleForm.gender")
                            el-radio-button(label="boy") 男生
                            el-radio-button(label="girl") 女生
                    el-form-item(label="系别" prop="classic")
                        el-input(v-model="ruleForm.phone")
                    el-form-item(label="专业" prop="phone")
                        el-input(v-model="ruleForm.phone")
                    el-form-item(label="QQ" prop="phone")
                        el-input(v-model="ruleForm.phone")
                    el-form-item(label="微信号" prop="phone")
                        el-input(v-model="ruleForm.phone")
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
        ]
      }
    }
  },
  methods: {
    // async function fetchSomething () {
    //   let ip = await this.$axios.$get('http://icanhazip.com');
    //   console.log(ip);
    // };
    async login() {
      let username = this.ruleForm.email
      let password = this.ruleForm.password
      let { data } = await axios.post(`/api/user/login`, {
        email: username,
        password: password
      })
      console.log(data.loginStatus === '0')
      if (data.loginStatus === '0') {
        console.log('wsl')
        this.$notify.error({
          title: '登录失败',
          message: data.errMsg
        })
      }
      if (data.loginStatus === '1') {
        this.$notify({
          title: '登录成功~',
          message:
            '身份验证通过，用户ID为' + data.userId + '，现在启动页面转向。',
          type: 'success',
          onClose: this.redirectpage
        })
      }
      console.log(data)
    },
    redirectpage() {
      window.location.href = '/home/portal'
    },
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.login()
          this.$notify.info({
            title: '登录中...',
            message: '连接服务器中'
          })
        } else {
          console.log('error submit!!')
          return false
        }
      })
    },
    resetForm(formName) {
      this.$refs[formName].resetFields()
    }
  }
}
</script>
