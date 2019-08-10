<template>
  <div class="detail">
    <!-- 面包屑 -->
    <el-breadcrumb separator-class="el-icon-arrow-right" class="breadcrumb">
      <el-breadcrumb-item :to="{ path: '/hotel' }">酒店</el-breadcrumb-item>
      <el-breadcrumb-item :to="{ path: '/hotel'}">南京酒店</el-breadcrumb-item>
      <el-breadcrumb-item>7天连锁酒店(南京长江大桥店)</el-breadcrumb-item>
    </el-breadcrumb>

    <!-- 标题 -->
    <el-row type="flex">
      <p class="title">7天连锁酒店(南京长江大桥店)</p>
    </el-row>
    <p>7 tian lian suo hotel (nan jing chang jiang da qiao dian)</p>
    <div>
      <i class="el-icon-location location"></i>
      <span>江山路与大桥北路交叉口东南角(江山路大桥北路公交站旁)</span>
    </div>

    <!-- 图片预览 -->
    <div class="preview">
      <el-row type="flex">
        <div class="preview_left">
          <img src="http://157.122.54.189:9093/images/hotel-pics/1.jpeg" alt />
        </div>
        <div class="preview_right">
          <img src="http://157.122.54.189:9093/images/hotel-pics/1.jpeg" alt />
          <img src="http://157.122.54.189:9093/images/hotel-pics/2.jpeg" alt />
          <img src="http://157.122.54.189:9093/images/hotel-pics/3.jpeg" alt />
          <img src="http://157.122.54.189:9093/images/hotel-pics/4.jpeg" alt />
          <img src="http://157.122.54.189:9093/images/hotel-pics/5.jpeg" alt />
          <img src="http://157.122.54.189:9093/images/hotel-pics/6.jpeg" alt />
        </div>
      </el-row>
    </div>

    <!-- 房间信息 -->
    <el-table :data="tableData" style="width: 100%">
      <el-table-column prop="date" label="价格来源" width="400"></el-table-column>
      <el-table-column prop="name" label="低价房型" width="400"></el-table-column>
      <el-table-column prop="address" label="最低价格/每晚">
        <template slot-scope="scope">
          <span :style="`color:#ff9900;padding-right:10px;`">{{scope.row.price}}</span>起
          <span class="el-icon-arrow-right" :style="`color:#ff9900;`"></span>
        </template>
      </el-table-column>
    </el-table>

    <!-- 地图信息 -->
    <el-row type="flex" class="mapMsg">
      <Map />
      <el-tabs v-model="activeName" @tab-click="handleClick" class="tabs">
        <el-tab-pane label="风景" name="first" class="scenery">
          <el-row type="flex" class="sceneryList">
            <el-col :span="18">高淳老街</el-col>
            <el-col :span="6">1.03公里</el-col>
          </el-row>
          <el-row type="flex" class="sceneryList">
            <el-col :span="18">高淳老街</el-col>
            <el-col :span="6">1.03公里</el-col>
          </el-row>
        </el-tab-pane>
        <el-tab-pane label="交通" name="second" class="scenery">
          <el-row type="flex" class="sceneryList">
            <el-col :span="18">高淳老街</el-col>
            <el-col :span="6">1.03公里</el-col>
          </el-row>
          <el-row type="flex" class="sceneryList">
            <el-col :span="18">高淳老街</el-col>
            <el-col :span="6">1.03公里</el-col>
          </el-row>
        </el-tab-pane>
      </el-tabs>
    </el-row>

    <!-- 酒店信息 -->
    <el-row type="flex" class="hotelMsg">
      <p>基本信息</p>
      <p>入住时间: 14:00之后</p>
      <p>离店时间: 12:00之前</p>
      <p>2010年开业 / 2015年装修</p>
      <p>酒店规模: 148间客房</p>
    </el-row>
    <el-row type="flex" class="hotelMsg">
      <p>主要设施</p>
      <span>外币兑换服务</span>
      <span>电梯</span>
      <span>洗衣服务</span>
      <span>热水壶</span>
    </el-row>
    <el-row type="flex" class="hotelMsg">
      <p>主要设施</p>
    </el-row>
    <el-row type="flex" class="hotelMsg">
      <p>主要设施</p>
    </el-row>

    <!-- 用户评论 -->
    <div class="comment">
      <h3>0条真实用户评论</h3>
      <el-row type="flex" class="comment_total">
        <el-col :span="4" class="total">
          <p>总评数：9</p>
          <p>好评数：1</p>
          <p>差评数：2</p>
        </el-col>
        <!-- 评分 -->
        <el-col :span="5">
          <el-rate v-model="value" disabled show-score text-color="#ff9900" score-template="{value}" class="score"></el-rate>
        </el-col>
        <el-col :span="3">
          <div class="text">环境</div>
        </el-col>
        <el-col :span="3">
          <div class="text">产品</div>
        </el-col>
        <el-col :span="3">
          <div class="text">服务</div>
        </el-col>
      </el-row>
    </div>
  </div>
</template>

<script>
import Map from "@/components/hotel/map.vue";
export default {
  components: {
    Map
  },
  data() {
    return {
      activeName: "first",
      value: 3.5,
      tableData: [
        {
          date: "携程",
          name: "高级大床房",
          price:'￥115',
        },
        {
          date: "2016-05-04",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1517 弄",
           price:'￥115',
        },
        {
          date: "2016-05-01",
          name: "王小虎",
          address: "上海市普陀区金沙江路 1519 弄",
           price:'￥115',
        }
      ]
    };
  },
  methods: {
    handleClick(tab, event) {
      console.log(tab, event);
    }
  }
};
</script>

<style lang="less" scoped>
.detail {
  width: 1000px;
  margin: 0 auto;
  color: #666;
  font-size: 14px;
  .breadcrumb {
    padding: 17px 0 25px;
  }
  .title {
    color: #333;
    font-size: 25px;
  }
  .location {
    font-size: 18px;
  }
  .preview {
    margin-top: 30px;
    margin-bottom: 50px;
    .preview_left {
      font-size: 0;
      img {
        width: 640px;
        height: 360px;
        cursor: pointer;
      }
    }
    .preview_right {
      margin-left: 8px;
      font-size: 0;
      img {
        cursor: pointer;
        margin-left: 16px;
        margin-bottom: 15px;
        width: 160px;
        height: 110px;
        &:nth-last-child(1) {
          margin-bottom: 0;
        }
        &:nth-last-child(2) {
          margin-bottom: 0;
        }
      }
    }
  }
  .roomPri {
    color: #f7ba2a;
  }
  .mapMsg {
    margin-top: 40px;
    margin-bottom: 50px;
    .tabs {
      width: 330px;
      margin-left: 20px;
      .scenery {
        height: 305px;
        overflow-y: scroll;
        .sceneryList {
          cursor: pointer;
          margin-bottom: 20px;
        }
      }
    }
  }
  .hotelMsg {
    padding: 20px 0;
    border-bottom: 1px solid #eee;
    p {
      width: 200px;
    }
    span {
      padding: 5px 10px;
      background-color: #eee;
      margin-right: 10px;
    }
  }
  .comment {
    margin-top: 40px;
    h3 {
      color: #000;
    }
    .comment_total {
      padding: 20px 0;
      font-size: 16px;
      .score {
        line-height: 74px;
      }
      .text {
        box-sizing:border-box;
        width: 70px;
        height: 70px;
        text-align: center;
        padding-top: 33px;
        color: #f7ba2a;
      }
    }
  }
}
</style>
