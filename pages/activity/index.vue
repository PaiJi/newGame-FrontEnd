<template lang="pug">
    el-container
        el-header(style="height:auto;")
            myHeader
        el-main
            section
                el-carousel
                    el-carousel-item
                        img(src="https://shouxiaji.cn/img/index-bg.jpg")
            section
                .club-list(v-cloak='')
                    el-row(:gutter='20')
                        el-col(:span='6' v-for="item in activityList.slice(0,4)" :key="item.id")
                            nuxt-link(v-bind:to="{name:'activity-id',params:{id:item.id}}")
                                el-card
                                    .img(:style="{backgroundImage: `url(`+item.imgUrl+`)`}")
                                    .card-info
                                        .card-title
                                            h3(style="display:inline-block") {{item.name}}
                                            el-tag(type="info") {{item.type}}
                                        .card-intro
                                            p {{item.intro}}
                    el-row(:gutter='20')
                        el-col(:span='6' v-for="item in activityList.slice(4,8)" :key="item.id")
                            nuxt-link(v-bind:to="{name:'activity-id',params:{id:item.id}}")
                                el-card
                                    .img(:style="{backgroundImage: `url(`+item.imgUrl+`)`}")
                                    .card-info
                                        .card-title(style="")
                                            h3(style="display:inline-block") {{item.name}}
                                            el-tag(type="info") {{item.type}}
                                        .card-intro
                                            p {{item.intro}}
</template>
<style lang="scss">
.club-list {
  padding: 20px 0px;

  .el-row {
    padding: 15px 0px;
    a {
      text-decoration: none;
    }
  }
  .el-card {
    height: 300px;
    .img {
      width: 100%;
      background-size: cover;
      height: 150px;
    }
  }
  .card-info {
    margin-top: 10px;
  }
  .card-title {
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .card-intro {
    margin-top: 10px;
    height: 50px;
    overflow: hidden;
  }
  [v-cloak] {
    visibility: hidden;
  }
}
</style>
<script>
import myHeader from '~/components/header.vue'
import axios from 'axios'
export default {
  components: {
    myHeader
  },
  data() {
    return {
      activityList: []
    }
  },
  mounted() {
    this.getClubList()
  },
  methods: {
    async getClubList() {
      console.log('START GET DATA')
      let { data } = await axios.get('/api/activity/activitylist')
      this.activityList = data
    }
  }
}
</script>
