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
            el-form(:model="ruleForm" :rules="rules" :hide-required-asterisk="true" ref="ruleForm" label-width="70px" label-position="right")
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
              el-form-item(label="年龄" prop="age")
                el-input(v-model="ruleForm.age" type="number")
              el-form-item(label="QQ" prop="qq")
                el-input(v-model="ruleForm.qq" type="number")
              el-form-item(label="微信" prop="wechat")
                el-input(v-model="ruleForm.wechat")
              el-form-item(label="学院" prop="department")
                el-select(v-model="ruleForm.department" placeholder="请选择学院")
                  el-option(label="智能与电子工程学院" value='1')
                  el-option(label="计算机与软件学院" value='2')
                  el-option(label="信息与商务管理学院" value='3')
                  el-option(label="数字艺术与设计学院" value='4')
                  el-option(label="外国语学院" value='5')
                  el-option(label="健康医疗科技学院" value='6')
                  el-option(label="高等职业技术学院" value='7')
                  el-option(label="国际教育学院" value='8')
                  el-option(label="继续教育学院" value='9')
              el-form-item(label="系部" prop="major")
                el-select(v-model="ruleForm.major" placeholder="请选择专业")
                  el-option(label="电子信息工程" value='1')
              el-form-item
                el-button(type="primary" style="width:100%" @click="submitForm('ruleForm')") 注册
</template>


<style lang="scss">
html {
  //height: 100%;
}
body {
  //height: 100%;
  // display: flex;
  // flex-direction: column;
  // justify-content: center;
}
.registerForm {
  padding: 20px;
  .registerImg {
  }
  @media (max-width: 800px) {
    .registerImg {
      display: none;
    }
  }
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
        password: '',
        phone: '',
        gender: '',
        realname: '',
        nickname: '',
        age: '',
        qq: '',
        wechat: '',
        department: '',
        major: ''
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
            message: '密码忘记填了！',
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
            message: '姓名忘记填了！',
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
            message: '昵称忘记填了！',
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
            message: '电话忘记填了！',
            trigger: 'blur'
          }
        ],
        gender: [
          {
            required: 'true',
            message: '性别是必选~'
          }
        ],
        age: [
          {
            required: 'true',
            message: '年龄忘记填了！'
          }
        ],
        qq: [
          {
            required: 'true',
            message: 'QQ忘记填了！'
          }
        ],
        wechat: [
          {
            required: 'true',
            message: '微信忘记填了！'
          }
        ],
        department: [
          {
            required: 'true',
            message: '学院忘记填了！'
          }
        ],
        major: [
          {
            required: 'true',
            message: '系部忘记填了！'
          }
        ]
      }
    }
  },
  methods: {
    async register() {
      let { data } = await axios.post(
        `http://localhost:2333/newGame-BackEnd/public/index.php/api/user/register`,
        {
          email: this.ruleForm.email,
          password: this.ruleForm.password,
          realname: this.ruleForm.realname,
          nickname: this.ruleForm.nickname,
          gender: this.ruleForm.gender,
          phone: this.ruleForm.phone,
          age: this.ruleForm.age,
          qq: this.ruleForm.qq,
          wechat: this.ruleForm.wechat,
          department: this.ruleForm.department,
          major: this.ruleForm.major
        }
      )
      console.log(data.registerStatus === '0')
      if (data.registerStatus === '0') {
        this.$notify.error({
          title: '注册失败',
          message: data.errMsg
        })
      }
      if (data.registerStatus === '1') {
        this.$notify({
          title: '注册成功',
          message: '重定向到登录页面',
          type: 'success'
        })
        this.$router.push({ path: '/user/login' })
      }
      //console.log(data)
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
          this.$notify.error({
            title: '提交不成功',
            message: '请完整填写注册表单'
          })
          return false
        }
      })
    }
  }
}
</script>
