<template lang="pug">
    el-container
        el-header(style="height:auto;")
            myHeader
        el-main
            section.clubInfo
                el-row
                    el-col
                        h3 {{clubDetail.name}}——纳新表单
                el-row.clubIntro
                    el-col
                        p {{clubDetail.intro}}
            section.questionArea
                el-form(:model="answerForm"  ref="form" label-position="top")
                    el-form-item(v-for="(item,index) in questionList" :label="index+1+'.'+item.msg" prop="answer")
                        el-input(v-if="item.type=='input'" v-model="answerForm.answerList[index].answer")
                        el-input(v-if="item.type=='number'" v-model="answerForm.answerList[index].answer")
                        el-radio-group(v-if="item.type=='radio'" v-model="answerForm.answerList[index].answer")
                            el-radio(v-for="(radio,index1) in JSON.parse(item.answer)" :label='radio' border) {{radio}}
                        el-checkbox-group(v-if="item.type=='checkbox'" v-model="answerForm.answerList[index].answer")
                            el-checkbox(v-for="(checkbox,index2) in JSON.parse(item.answer)" :label='checkbox' border)
                    el-form-item
                        el-button(type="primary" @click="submitForm") 提交表单

</template>
<style lang="scss">
section.clubInfo {
  margin: 20px 0px;
  h3 {
    font-size: 22px;
    color: #303133;
  }
  .clubIntro {
    margin: 20px 0px;
    color: #606266;
  }
}
</style>

<script>
import axios from 'axios'
// import sortablejs from 'sortablejs'
// import draggable from 'vuedraggable'
//import $ from 'jquery'
import myHeader from '~/components/header.vue'
export default {
  components: {
    myHeader
    // sortablejs,
    // draggable
  },
  data() {
    return {
      paramValue: '',
      clubDetail: [],
      userId: myHeader.data().userId,
      questionList: [],
      answerForm: {
        answerList: []
      }
    }
  },
  mounted() {
    this.paramValue = this.$route.params.id
    this.getClubDetail()
    this.fetchQuestion()
  },
  methods: {
    async getClubDetail() {
      let { data } = await axios.get(
        '/api/club/getclubdetail?clubid=' + this.paramValue
      )
      this.clubDetail = data
    },
    async joinClub() {
      let { data } = await axios.get(
        '/api/club/joinclub?clubid=' + this.paramValue
      )
      console.log(data)
      if (data.joinClubResultCode === '1') {
        //弹窗告知用户已经加入
        this.$notify({
          title: '成功加入社团辣',
          message: '您已经是社团成员了，快去看看吧！',
          type: 'success'
        })
      }
      if (data.joinClubResultCode === '0' && data.clubRequest === '1') {
        //告诉用户需要填表
        this.$notify.info({
          title: '需要填写表单',
          message: data.errMsg
        })
      }
    },
    async fetchQuestion() {
      let { data } = await axios.get(
        '/api/club/getquestion?clubId=' + this.paramValue
      )
      //console.log(data)
      this.questionList = data
      for (let index = 0; index < data.length; index++) {
        var t = {}
        t['question_id'] = data[index].id
        t['question'] = data[index].msg
        t['type'] = data[index].type
        if (data[index].type == 'checkbox') {
          t['answer'] = []
        } else {
          t['answer'] = ''
        }
        this.answerForm.answerList.push(t)
      }
    },
    async submitForm() {
      let vaildNum = 0
      for (const question of this.answerForm.answerList) {
        if (question.answer == '') {
          this.$notify.error({
            title: '请填写完整所有内容！',
            message: '问题：“' + question.question + '”未填写。'
          })
        } else {
          vaildNum = vaildNum + 1
        }
        console.log(vaildNum)
      }
      if (vaildNum == this.answerForm.answerList.length) {
        let { data } = await axios.post(
          '/api/club/submitApplyForm?clubId=' + this.paramValue,
          {
            answerList: this.answerForm.answerList
          }
        )
        if (data.submitApplyFormResultCode == '1') {
          this.$notify({
            title: '成功提交表单',
            message: '请耐心等待管理员审核~',
            type: 'success'
          })
        }
        if (data.submitApplyFormResultCode == '0') {
          this.$notify.error({
            title: '有错误发生',
            message: data.errMsg
          })
        }
      }
    }
  }
}
</script>
