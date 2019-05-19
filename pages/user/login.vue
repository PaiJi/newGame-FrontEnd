<template lang="pug">
  el-container
    el-header(style="height:auto;")
      myHeader
    el-main
      el-row(:gutter='20' type='flex' justify="center").registerForm
        //el-col(:lg='8' :md='8' :sm='8').registerImg
          .bg-purple
        el-col(:lg='8' :md='9' :sm='10')
          .grid-content
            el-form(:model="ruleForm" :rules="rules" :hide-required-asterisk="true" ref="ruleForm" label-width="100px")
              el-form-item(label="邮箱地址" prop="email")
                el-input(v-model="ruleForm.email" type="email")
              el-form-item(label="密码" prop="password")
                el-input(v-model="ruleForm.password" type="password")
              el-form-item
                <el-button type="primary" @click="submitForm('ruleForm')">登录</el-button>
                <el-button @click="$router.push({ path: '/user/register' })">注册</el-button>
</template>
<style lang="scss">
</style>

<script>
import axios from 'axios'
import myHeader from '~/components/header.vue'
export default {
  components: {
    myHeader
  },
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
          {
            required: true,
            message: '密码忘记填了！',
            trigger: 'blur'
          },
          {
            min: 6,
            message: '太短啦！再长点'
          }
        ]
      }
    }
  },
  methods: {
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
      this.$router.push({ path: '/home/portal' })
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
