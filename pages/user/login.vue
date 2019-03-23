<template>
  <div class="container">
    <div class="logo"/>
    <div class="input-area">
      <el-form
        ref="ruleForm"
        :model="ruleForm"
        :rules="rules"
        :hide-required-asterisk="true"
        label-width="100px"
        class="demo-ruleForm"
      >
        <el-form-item label="邮箱地址" prop="email">
          <el-input v-model="ruleForm.email"/>
        </el-form-item>
        <el-form-item label="密码" prop="password">
          <el-input v-model="ruleForm.password" type="password"/>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitForm('ruleForm')">登录</el-button>
          <el-button @click="fetchSomething()">注册</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>
<style lang="scss">
html {
  height: 100%;
}
body {
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: center;
}
.container {
  width: 400px;
  height: 400px;
  margin: 0 auto;
  border: 1px solid #dadce0;
  border-radius: 8px;
}
.logo {
  //background-image: url("~assets/img/login.png");
  height: 200px;
  background: url('~assets/img/login.png');
  background-size: cover;
}
.input-area {
  padding: 20px;
}
.login-input {
  width: auto;
  padding: 0px 10px 0px 10px;
}
.el-col {
  border-radius: 4px;
}
</style>

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
