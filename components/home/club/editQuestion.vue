<template lang="pug">
    .container
        .Intro
            p 您当前正在编辑本社团纳新问卷，每一项均可拖动，修改数据后请记得点击保存。
        .control-area
            el-button(type="primary" plain @click="addQuestion") 新增问题
            el-button(type="success" plain @click="saveQuestion") 保存问卷
        el-collapse(v-model="currentItem" accordion)
            draggable(v-model="questionList")
                transition-group
                    div(v-for="(item,index) in questionList" :key='item.id')
                        el-collapse-item(:title="item.msg" :name="item.id")
                            el-row
                                p 问题/内容：
                            el-row
                                el-input(v-model="questionList[index].msg")
                            el-row
                                p 类型：
                            el-row
                                el-select(v-model="item.type" @change="handleTypeChange($event,index)")
                                    el-option(label="单选框" value="radio")
                                    el-option(label="数字" value="number")
                                    el-option(label="多选框" value="checkbox")
                                    el-option(label="单行文本" value="input")
                                    el-option(label="多行文本" value="textarea")
                            el-row(v-if="item.type=='input'")
                                //el-col
                                    el-input(v-model="questionList[index].answer")
                            el-row(v-if="item.type=='number'")
                                //el-col
                                    el-input(v-model="questionList[index].answer")
                            el-row(v-if="item.type=='radio'")
                                div(v-if="questionList[index].answer.length!=0")
                                    el-row
                                        p 选项：
                                    el-row(:gutter='20')
                                        el-col(v-for="(radio,index1) in item.answer" :md='3' :key="index1")
                                            //el-radio(v-model="questionList[index].answer" :label='radio' border) {{radio}}
                                            el-input(v-model="questionList[index].answer[index1]")
                                .radio-button
                                    el-button(@click="questionList[index].answer.push('')") 新增
                                    el-button(@click="questionList[index].answer.pop('')") 删除
                            el-row(v-if="item.type=='checkbox'")
                                div(v-if="questionList[index].answer")
                                    el-row
                                        p 选项
                                    el-row(:gutter='20')
                                        el-col(v-for="(checkbox,index2) in item.answer" :md='3' :key="index2")
                                            el-input(v-model="questionList[index].answer[index2]")
                                    div.radio-button
                                            el-button(@click="questionList[index].answer.push('')") 新增
                                            el-button(@click="questionList[index].answer.pop('')" type="warning") 删除
                            el-row
                                el-col(style="text-align:right;")
                                    el-button(type="danger" plain @click="deleteQuestion(item.id,index)") 删除问题
</template>
<style lang="scss" scoped>
.control-area {
  margin: 10px 0px;
}
.radio-button {
  margin: 10px 0px;
}
</style>
<script>
import axios from 'axios'
import draggable from 'vuedraggable'
export default {
  components: {
    draggable
  },
  props: {
    selectClub: {
      type: Number,
      default: 0
    }
  },
  data() {
    return {
      currentItem: '',
      questionList: []
    }
  },
  mounted() {
    this.fetchQuestionList()
  },
  methods: {
    async fetchQuestionList() {
      let { data } = await axios.get(
        '/api/club/getquestion?clubId=' + this.selectClub
      )
      //console.log(data)
      this.questionList = data.map(item => {
        // eslint-disable-next-line prettier/prettier
        if (item.type == 'checkbox' || item.type == 'radio') {
          //console.log(item.answer)
          try {
            item.answer = JSON.parse(item.answer)
          } catch (e) {
            console.log(e)
          }
        }
        return item
      })
    },
    // handleChange(val) {
    //   //console.log(val)
    // },
    addQuestion() {
      let que = new Object()
      que.id = Math.floor(Math.random() * 100000)
      que.type = 'input'
      que.msg = '请修改默认信息'
      que.answer = []
      que.required = 1
      this.questionList.push(que)
    },
    async deleteQuestion(quesitonId, num) {
      console.log(num)
      let { data } = await axios.post(
        '/api/club/deletequestion?clubId=' + this.selectClub,
        {
          questionId: quesitonId
        }
      )
      if (data.DeleteQuestionResultCode == '1') {
        this.$notify({
          title: '删除成功',
          message: '该问题已被删除',
          type: 'success'
        })
      } else {
        this.$notify.error({
          title: '删除错误',
          message: data.errMsg
        })
      }
      this.questionList.splice(num, 1)
    },
    async saveQuestion() {
      for (let index = 0; index < this.questionList.length; index++) {
        this.questionList[index].sort = index
      }
      let { data } = await axios.post(
        '/api/club/updatequestion?clubId=' + this.selectClub,
        {
          data: this.questionList
        }
      )
      ///console.log(data)
      if (data.queryResult == '1') {
        this.$notify({
          title: '保存成功',
          message: '表单已更新',
          type: 'success'
        })
      } else {
        this.$notify.error({
          title: '保存失败',
          message: data.errMsg
        })
      }
    },
    addOption(index) {
      this.questionList[index].answer.push('')
    },
    handleTypeChange(val, index) {
      console.log(val)
      console.log(index)
      if (val == 'radio' || val == 'checkbox') {
        this.questionList[index].answer = []
      } else {
        this.questionList[index].answer = ''
      }
    }
  }
}
</script>
