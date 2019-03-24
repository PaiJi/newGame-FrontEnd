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
        el-footer
</template>


<script>
import Logo from '~/components/Logo.vue'
import axios from 'axios'
export default {
  components: {
    Logo
  },
  data() {
    return {
      clubData: []
    }
  },
  created() {
    //this.getClubList()
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
.el-header {
  //display: flex;
  //align-items: center;
  background-color: #fafafa;
  .header-left {
    display: flex;
    align-items: center;
  }
  .intro {
    h1,
    h3 {
      padding: 3px 0px;
      color: #111;
      font-family: 'SF Pro SC', 'SF Pro Text', 'SF Pro Icons', 'PingFang SC',
        'Helvetica Neue', 'Helvetica', 'Arial', sans-serif;
    }
  }
  .el-row {
    display: flex;
    align-items: center;
  }
  .button-group {
    margin: 0 auto;
  }
}
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
  }
  [v-cloak] {
    visibility: hidden;
  }
}
.container {
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
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

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
