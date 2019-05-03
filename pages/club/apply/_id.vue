<template lang="pug">
    el-container
        el-header(style="height:auto;")
            myHeader
        el-main
            section.clubInfo
                el-row
                    el-col
                        h3 XX社团纳新表单
            section.questionArea
                .questionRow(v-for="(item,index) in questionList")
                    el-row(v-if="item.type=='input'")
                        el-col
                            p {{item.msg}}
                        el-col
                            el-input(v-model="answerList[index].answer")
                    el-row(v-if="item.type=='number'")
                        el-col
                            p {{item.msg}}
                        el-col
                            el-input(v-model="answerList[index].answer")
                    el-row(v-if="item.type=='radio'")
                        el-col
                            p {{item.msg}}
                        el-col(v-for="(radio,index1) in JSON.parse(item.answer)" :md='3' :key="index1")
                            el-radio(v-model="answerList[index].answer" :label='radio' border) {{radio}}
                    el-row(v-if="item.type=='checkbox'")
                        el-col
                            p {{item.msg}}
                        el-checkbox-group(v-model="answerList[index].answer")
                            el-col(v-for="(checkbox,index2) in JSON.parse(item.answer)" :md='3' :key="index2")
                                el-checkbox(:label='checkbox' border)

</template>
<style lang="scss">
</style>

<script>
import axios from 'axios'
import sortablejs from 'sortablejs'
import draggable from 'vuedraggable'
//import $ from 'jquery'
import myHeader from '~/components/header.vue'
export default {
  components: {
    myHeader,
    sortablejs,
    draggable
  },
  data() {
    return {
      paramValue: '',
      clubDetail: [],
      userId: myHeader.data().userId,
      questionList: [],
      answerList: []
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
      console.log(data)
      this.questionList = data
      for (let index = 0; index < data.length; index++) {
        var t = {}
        t['question'] = data[index].msg
        if (data[index].type == 'checkbox') {
          t['answer'] = []
        } else {
          t['answer'] = ''
        }
        this.answerList.push(t)
      }
    }
  }
}
</script>
