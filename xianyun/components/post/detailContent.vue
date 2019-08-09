<template>
    <div class="detailContent">
      <div  v-for="(item, index) in data.data" :key="index">
        <h1 class="title">{{item.title}}</h1>
        <div class="postInfo">
          <span>攻略：{{item.created_at | dateFormat}}</span>
          <span>阅读：{{item.watch}}</span>
        </div>
        <div class="content" v-html="item.content">
        </div>

        <div class="share">
          <el-row type="flex">
            <el-col class="mouseHover">
                <i class="el-icon-edit-outline"></i>
                <p>评论({{item.comments.length}})</p>
            </el-col>
            <el-col class="mouseHover">            
              <i class="el-icon-star-off" @click="handleStar"></i>
              <p @click="handleStar">收藏</p>
            </el-col>
            <el-col class="mouseHover">            
              <i class="el-icon-share"></i>
              <p>分享</p>
            </el-col>
            <el-col class="mouseHover">            
              <i class="el-icon-magic-stick" @click="handleLike"></i>
              <p @click="handleLike">点赞({{item.like?item.like:0}})</p>
            </el-col>
          </el-row>
        </div>
        </div>
    </div>
</template>

<script>
import moment from 'moment'
export default {
    data(){
        return{
          
        }
    },

    props:{
      data:{
        type:Object,
        default:{}
      },

      id:{
        type:String,
        default:""
      }
    },

    filters: {
      dateFormat(value){

        return moment(value).format("YYYY-MM-DD")
      }
    },

    methods: {
      handleStar(){
        // console.log(this.id);
        this.$axios({
          url:'/posts/star',
          params:{
            id:this.id
          },
          headers:{
            Authorization: `Bearer ${this.$store.state.user.userInfo.token}`
          }
        }).then(res=>{
          // console.log(res);
          if(res.data.message == "收藏成功"){
            this.$message.success(res.data.message)
          }else{
            this.$message.warning(res.data.message)
          }
        })
      },

      handleLike(){
        this.$axios({
          url:'/posts/like',
          params:{
            id:this.id
          },
          headers:{
            Authorization: `Bearer ${this.$store.state.user.userInfo.token}`
          }
        }).then(res=>{
          // console.log(res);
          if(res.data.message == "点赞成功"){
            this.$message.success(res.data.message)
          }else{
            this.$message.warning(res.data.message)
          }
        })
      }
    },

    mounted () {

    }
}
</script>

<style scoped>
.title {
    padding: 20px 0;
}
.postInfo {
    color: #999;
    text-align: right;
    border-top: #ccc 1px solid;
    padding: 20px;
}
.content /deep/ img{
    max-width: 700px!important;
}
.share{
    box-sizing: border-box;
    height: 140px;
    padding:40px 180px 34px 180px;
    text-align: center;
    font-size:28px;
    color:#ffa500;
}
.mouseHover{
    cursor: pointer;
}
.share p{
    font-size:14px;
    color:#999;
}
</style>
