<template>
    <div>
        <div  class="rightInfo"  v-for="(item,index) in recommend.data" :key="index">
            <nuxt-link :to="`/posts/detail?id=${item.id}`">
            <el-row type="flex">
                <el-col :span="8" class="leftBody"><img :src="item.images[0]" alt=""></el-col>
                <el-col :span="16" class="rightBody">
                    <div class="title">{{item.title}}</div>
                    <p>{{item.created_at | timeFormat}} 阅读 {{item.watch}}</p>
                </el-col>
            </el-row>
            </nuxt-link>
        </div>
    </div>
</template>

<script>
import moment from 'moment'
export default {
    data(){
        return{
            recommend:{

            }
        }
    },

    props:{
        data:{
            type:Object,
            default:{}
        }
    },
    mounted () {
        this.$axios({
            url:"/posts/recommend",
        }).then(res => {
            // console.log(res);
            this.recommend = res.data
            console.log(this.recommend);
        })
    },
    filters: {
        timeFormat(value){
            return moment(value)
        }
    }
}
</script>

<style scoped>
.rightInfo{
    padding:20px 0;
    border-bottom: 1px solid #ddd;
}
.rightInfo img{
    width: 100px;
    height: 80px;
}
.leftBody /deep/ img{
    width: 100%;
}
.rightBody{
    position: relative;
    padding-left: 10px;
}
.rightBody p{
    position: absolute;
    bottom: 0;
    right: 0;
    font-size: 12px;
    color: #999;
}
.title{
    height: 60px;
    overflow: hidden;
    font-size: 14px;
}
</style>
