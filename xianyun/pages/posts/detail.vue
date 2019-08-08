<template>
  <div id="detail">
    <el-row>
      <el-col :span="17">
        <el-breadcrumb separator="/">
          <el-breadcrumb-item :to="{ path: '/' }">旅游攻略</el-breadcrumb-item>
          <el-breadcrumb-item>攻略详情</el-breadcrumb-item>
        </el-breadcrumb>

        <!-- 正文组件 -->
        <DetailContent :data="detailInfo"/>

        <!-- 评论组件 -->
        <DetailComment />

        <!-- 评论列表组件 -->
        <DetailCommeentList />

      </el-col>

      <el-col :span="7" class="colright">
        <h4>相关攻略</h4>

        <!-- 侧边栏组件 -->
        <DetailSidebar :data="detailInfo"/>
        
      </el-col>
    </el-row>
  </div>
</template>

<script>
import DetailContent from '@/components/post/detailContent.vue'
import DetailComment from '@/components/post/detailComment.vue'
import DetailCommeentList from '@/components/post/detailCommeentList.vue'
import DetailSidebar from '@/components/post/detailSidebar.vue'
export default {
  data() {
    return {
      detailInfo: {},
    }
  },

  methods: {
    getData(){
      let { id } = this.$route.query

      // 获取文章数据
      this.$axios({
        url:`/posts/`,
        params:{ id }
      }).then(res => {
        // console.log(res.data);
        this.detailInfo = res.data;
      })
    }
  },

  mounted() {
    this.getData()
  },

  components: {
    DetailContent,
    DetailComment,
    DetailCommeentList,
    DetailSidebar
  },

  watch: {
    $route(){
      this.getData()
    }
  }
}
</script>

<style lang="less" scoped>
.clearfix::before,		
.clearfix::after{
  content:"";
  display: table;
}
.clearfix::after {
clear:both;
}
#detail {
  padding: 20px;
  margin: 0 auto;
  width: 1000px;

  .colright{
    padding-left: 20px;
    h4{
    font-weight: 400;
    font-size: 18px;
    padding-bottom: 10px;
    border-bottom: 1px solid #ddd;
    }
  }
}
</style>
