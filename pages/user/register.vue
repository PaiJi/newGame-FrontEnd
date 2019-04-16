<template lang="pug">
el-row(:gutter='20' type='flex' justify="center")
  el-col(:span='6')
    .bg-purple
  el-col(:span='6')
    .grid-content
      el-form(:model="ruleForm" :rules="rules" :hide-required-asterisk="true" ref="ruleForm" label-width="70px")
        el-form-item(label="邮箱地址" prop="email")
          el-input(v-model="ruleForm.email" type="email")
        el-form-item(label="密码" prop="password")
          el-input(v-model="ruleForm.password" type="password")
        el-form-item(label="姓名" prop="realname")
          el-input(v-model="ruleForm.realname")
        el-form-item(label="昵称" prop="nickname")
          el-input(v-model="ruleForm.nickname")
        el-form-item(label="手机号" prop="phone")
          el-input(v-model="ruleForm.phone")
        el-form-item(label="性别" prop="gender")
          el-radio-group(v-model="ruleForm.gender")
            el-radio-button(label="boy") 男生
            el-radio-button(label="girl") 女生
        el-form-item
          el-button(type="primary" style="width:100%" @click="submitForm('ruleForm')") 注册


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
.el-row {
  margin-bottom: 20px;
  &:last-child {
    margin-bottom: 0;
  }
}
.el-col {
  border-radius: 4px;
}
.bg-purple-dark {
  background: #99a9bf;
}
.bg-purple {
  background: #d3dce6;
  height: 100%;
  border-radius: 5px;
}
.bg-purple-light {
  background: #e5e9f2;
}
.grid-content {
  border-radius: 4px;
  min-height: 36px;
}
.row-bg {
  padding: 10px 0;
  background-color: #f9fafc;
}
</style>
<script>
import axios from 'axios'
export default {
  data() {
    return {
      ruleForm: {
        email: '',
        password: '',
        phone: '',
        gender: '',
        realname: '',
        nickname: ''
      },
      rules: {
        email: [
          {
            type: 'email',
            required: true,
            message: '邮箱格式不正确',
            trigger: ['change', 'blur']
          }
        ],
        password: [
          {
            required: true,
            message: '记得写这里鸭',
            trigger: 'blur'
          },
          {
            min: 6,
            message: '太短啦！再长点'
          }
        ],
        realname: [
          {
            required: true,
            message: '记得写这里鸭',
            trigger: 'blur'
          },
          {
            min: 2,
            message: '啥？你说你姓名就一个字？'
          }
        ],
        nickname: [
          {
            required: true,
            message: '记得写这里鸭',
            trigger: 'blur'
          },
          {
            max: 24,
            message: '太长了，程序会坏掉的！'
          }
        ],
        phone: [
          {
            required: 'true',
            message: '记得写这里鸭',
            trigger: 'blur'
          }
        ],
        gender: [
          {
            required: 'true',
            message: '性别是必选~'
          }
        ]
      }
    }
  },
  methods: {
    async register() {
      let username = this.ruleForm.email
      let password = this.ruleForm.password
      let realname = this.ruleForm.realname
      let nickname = this.ruleForm.nickname
      let gender = this.ruleForm.gender
      let phone = this.ruleForm.phone
      let { data } = await axios.post(
        `http://localhost:2333/newGame-BackEnd/public/index.php/api/user/register`,
        {
          email: username,
          password: password,
          realname: realname,
          nickname: nickname,
          gender: gender,
          phone: phone
        }
      )
      console.log(data.registerStatus === '0')
      if (data.registerStatus === '0') {
        console.log('wsl')
        this.$notify.error({
          title: '注册失败',
          message: data.errMsg
        })
      }
      if (data.registerStatus === '1') {
        this.$router.push({ path: '/user/login' })
      }
      console.log(data)
    },
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.register()
          this.$notify.info({
            title: '注册中...',
            message: '努力连接服务器'
          })
        } else {
          console.log('error submit!!')
          return false
        }
      })
    }
  }
}
</script>
