<template>
  <div class="post">
    <!-- 左侧热门城市 -->
    <div class="post-left"></div>
    <!-- 右侧布局 -->
    <div class="post-wrapper">
      <!-- input输入框 -->
      <el-input placeholder="请输入想去的地方，比如：'广州'"></el-input>
      <i class="el-icon-search"></i>
      <!-- 推荐 北上广 -->
      <div class="search-recommend">
        <i>推荐：</i>
        <span>广州</span>
        <span>上海</span>
        <span>北京</span>
      </div>
      <!-- 推荐攻略 -->
      <div class="post-title">
        <h4>推荐攻略</h4>
        <el-button type="primary" round>
          <i class="el-icon-edit"></i> 写游记
        </el-button>
      </div>
      <!-- 渲染内容 -->
      <el-divider></el-divider>
      <div class="post-content">
        <!-- 左侧图片 -->
        <el-card shadow="hover">
          <el-image
            style="width:100%"
            src="http://p1-q.mafengwo.net/s11/M00/B4/92/wKgBEFt6ZqaAJeK7AAbj58wpNlY06.jpeg?imageView2%2F2%2Fw%2F1360%2Fq%2F90"
          ></el-image>
        </el-card>
        <!-- 右侧内容 -->
        <div class="post-content-right">
          <p>小蛮腰广州塔，怎么玩才不遗憾？</p>
          <p>造型独特的“小蛮腰”广州塔，犹如一颗璀璨的明珠屹立在珠江边，成为羊城新八景之一。别看她“身材苗条”，塔内可是蕴藏了丰富的休闲玩乐，等待您去掀开她的面纱。这座直逼云霄的高塔，如何在有限的时间打卡不同楼层？</p>
          <!-- 底层 -->
          <el-row>
            <span class="el-item">
              <i class="el-icon-location-information"></i>
              <span>广州市</span>
            </span>
            <!-- 头像 -->
            <span class="el-item">
              by
              <i class="el-icon-s-custom"></i>
              <span style="color:orange">玩转地球者</span>
            </span>
            <span class="el-item">
              <i class="el-icon-view"></i>
              <span>188</span>
            </span>
            <h5>
              <span>3</span>赞
            </h5>
          </el-row>
        </div>
      </div>
      <!-- 分页 -->
      <div class="block">
        <el-pagination
          @size-change="handleSizeChange"
          @current-change="handleCurrentChange"
          :current-page="currentPage4"
          :page-sizes="[3, 5, 8, 10]"
          :page-size="100"
          layout="total, sizes, prev, pager, next, jumper"
          :total="40"
        ></el-pagination>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentPage4: 4
    };
  },
  methods: {
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
    }
  }
};
</script>

<style lang="less" scoped>
.post {
  display: flex;
  width: 1000px;
  margin: 30px auto;
  .post-left {
    width: 260px;
    height: 200px;
    background: red;
  }
  //右侧布局
  .block {
    margin-top: 30px;
  }
  .post-wrapper {
    position: relative;
    width: 700px;
    margin-left: 30px;
    // 搜素图标
    /deep/ .el-icon-search {
      position: absolute;
      top: 8px;
      left: 650px;
      font-size: 30px;
      color: orange;
      text-indent: 10px;
    }
    /deep/ .el-input {
      box-sizing: border-box;
      border: 3px solid orange;
      .el-input__inner {
        border: 0;
      }
    }
    .search-recommend {
      margin-top: 10px;
      margin-bottom: 20px;
      > span,
      i {
        font-size: 12px;
        color: #ccc;
      }
      > span:hover {
        text-decoration: underline;
        color: orange;
        cursor: pointer;
      }
    }
    // 推荐攻略
    .post-title {
      display: flex;
      justify-content: space-between;
      > h4 {
        font-size: 18px;
        font-weight: 400;
        color: orange;
        position: relative;
      }
      > h4::after {
        display: block;
        content: "";
        width: 72px;
        height: 3px;
        background: orange;
        position: absolute;
        bottom: 0px;
        left: 0px;
      }
    }
    // 主要内容
    .el-divider {
      margin: 0;
    }
    .post-content {
      display: flex;
      margin-top: 20px;
      /deep/ .el-card {
        box-sizing: border-box;
        width: 220px;
        height: 150px;
        .el-card__body {
          padding: 0;
        }
      }
      .post-content-right {
        width: 470px;
        margin-left: 10px;

        > p:nth-child(1) {
          margin-top: 5px;
          margin-bottom: 10px;
          font-size: 18px;
          cursor: pointer;
        }
        p:nth-child(1):hover {
          color: orange;
        }
        p:nth-child(2) {
          cursor: pointer;
          height: 63px;
          margin-bottom: 15px;
          overflow: hidden;
          color: rgba(0, 0, 0, 0.705);
        }
        /deep/.el-row {
          display: flex;
          font-size: 12px;
          color: #999;
          .el-item {
            margin-right: 15px;
          }
          h5 {
            color: orange;
            font-size: 14px;
            margin-left: 200px;
          }
        }
      }
    }
  }
}
</style>

