<template lang="pug">
    .container
        el-row(:gutter="20")
            el-col(:span="8")
                el-card
                    h5 我的社团
                    span 5
            el-col(:span="8")
                el-card
                    h5 我的活动
                    span 2
            el-col(:span="8")
                el-card
                    h5 设计中
                    span COMING SOON
        el-row
            el-col
                el-carousel(type="card" :interval="4000")
                    el-carousel-item
                    el-carousel-item
                    el-carousel-item
        el-row
            el-col
                h5 最新社团信息
        el-row
            el-col
                el-table
                    el-table-column(label="社团名称" width="")
                    el-table-column(label="社团简介" width="")
                    el-table-column(label="社团认数" width="")
                    el-table-column(label="注册时间" width="")

</template>
