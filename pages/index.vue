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
                        el-col(:span='6' v-for="item in clubData.slice(0,4)" :key="item.id")
                            nuxt-link(v-bind:to="{name:'club-id',params:{id:item.id}}")
                                el-card
                                    .img(:style="{backgroundImage: `url(`+item.img_logo+`)`}")
                                    .card-info
                                        .card-title
                                            h3(style="display:inline-block") {{item.name}}
                                            el-tag(type="info") 技术类
                                        .card-intro
                                            p {{item.intro}}
                    el-row(:gutter='20')
                        el-col(:span='6' v-for="item in clubData.slice(4,8)" :key="item.id")
                            nuxt-link(v-bind:to="{name:'club-id',params:{id:item.id}}")
                                el-card
                                    .img(:style="{backgroundImage: `url(`+item.img_logo+`)`}")
                                    .card-info
                                        .card-title(style="")
                                            h3(style="display:inline-block") {{item.name}}
                                            el-tag(type="info") 技术类
                                        .card-intro
                                            p {{item.intro}}
            section
                el-row
                    el-col(style="text-align:center")
                        el-button(round type="primary") 更多社团
                        el-button(round type="success") 看看活动
</template>


<script>
import myHeader from '~/components/header.vue'
import axios from 'axios'
export default {
  components: {
    myHeader
  },
  data() {
    return {
      clubData: []
    }
  },
  mounted() {
    this.getClubList()
  },
  methods: {
    async getClubList() {
      console.log('START GET DATA')
      let { data } = await axios.get('/api/club/clublist')
      this.clubData = data
    }
  }
}
</script>

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

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}
</style>
