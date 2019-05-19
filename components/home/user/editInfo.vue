<template lang="pug">
    el-row(:gutter='20' )
        el-col(:span='10')
            .grid-content
                el-form(:model="ruleForm" :rules="rules" :hide-required-asterisk="true" ref="ruleForm" label-width="70px")
                    el-form-item(label="姓名" prop="username")
                        el-input(v-model="ruleForm.username")
                    el-form-item(label="昵称" prop="nickname")
                        el-input(v-model="ruleForm.nickname")
                    el-form-item(label="年龄" prop="age")
                        el-input(v-model="ruleForm.age")
                    el-form-item(label="手机号" prop="phone")
                        el-input(v-model="ruleForm.phone")
                    el-form-item(label="性别" prop="gender")
                        el-radio-group(v-model="ruleForm.gender")
                            el-radio-button(label="boy") 男生
                            el-radio-button(label="girl") 女生
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
                    el-form-item(label="QQ" prop="qq")
                        el-input(v-model="ruleForm.qq")
                    el-form-item(label="微信号" prop="wechat")
                        el-input(v-model="ruleForm.wechat")
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
        password: '',
        phone: '',
        gender: '',
        username: '',
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
        username: [
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
  mounted() {
    this.getUserInfo()
  },
  methods: {
    async getUserInfo() {
      let { data } = await axios.get('/api/user/getuserinfo')
      console.log(data.loginStatus == true)
      if (data.loginStatus == 'true') {
        this.ruleForm = data.userInfo
      }
    },
    async updateInfo() {
      let { data } = await axios.post(`/api/user/updateInfo`, {
        realname: this.ruleForm.username,
        nickname: this.ruleForm.nickname,
        gender: this.ruleForm.gender,
        phone: this.ruleForm.phone,
        age: this.ruleForm.age,
        qq: this.ruleForm.qq,
        wechat: this.ruleForm.wechat,
        department: this.ruleForm.department,
        major: this.ruleForm.major
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
