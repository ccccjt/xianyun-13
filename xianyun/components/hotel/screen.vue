<template>
  <div class>
    <el-row class="block-col-2 screen01" type="flex">
      <el-col :span="8" class="elright" justify="center">
        <span class="demonstration price">价格</span>
        <span class="demonstration">0-4000</span>
        <el-slider v-model="value2"></el-slider>
      </el-col>
      <el-col :span="4" class="elleft">
        <span class="demonstration">住宿等级</span>
        <el-dropdown>
          <span class="el-dropdown-link">
            不限
            <i class="el-icon-arrow-down el-icon--right"></i>
          </span>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item icon="el-icon-plus">黄金糕</el-dropdown-item>
            <el-dropdown-item icon="el-icon-circle-plus">狮子头</el-dropdown-item>
            <el-dropdown-item icon="el-icon-circle-plus-outline">螺蛳粉</el-dropdown-item>
            <el-dropdown-item icon="el-icon-check">双皮奶</el-dropdown-item>
            <el-dropdown-item icon="el-icon-circle-check">蚵仔煎</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </el-col>
      <el-col :span="4" class="elleft" justify="center">
        <span class="demonstration">住宿类型</span>
        <el-dropdown>
          <span class="el-dropdown-link">
            不限
            <i class="el-icon-arrow-down el-icon--right"></i>
          </span>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item icon="el-icon-plus">黄金糕</el-dropdown-item>
            <el-dropdown-item icon="el-icon-circle-plus">狮子头</el-dropdown-item>
            <el-dropdown-item icon="el-icon-circle-plus-outline">螺蛳粉</el-dropdown-item>
            <el-dropdown-item icon="el-icon-check">双皮奶</el-dropdown-item>
            <el-dropdown-item icon="el-icon-circle-check">蚵仔煎</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </el-col>
      <el-col :span="4" class="elleft">
        <span class="demonstration">酒店设施</span>
        <el-dropdown>
          <span class="el-dropdown-link">
            不限
            <i class="el-icon-arrow-down el-icon--right"></i>
          </span>
          <el-dropdown-menu slot="dropdown">
            <el-dropdown-item icon="el-icon-plus">黄金糕</el-dropdown-item>
            <el-dropdown-item icon="el-icon-circle-plus">狮子头</el-dropdown-item>
            <el-dropdown-item icon="el-icon-circle-plus-outline">螺蛳粉</el-dropdown-item>
            <el-dropdown-item icon="el-icon-check">双皮奶</el-dropdown-item>
            <el-dropdown-item icon="el-icon-circle-check">蚵仔煎</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </el-col>
      <el-col :span="4" class="elleft">
        <span class="demonstration">酒店品牌</span>
        <el-dropdown>
          <span class="el-dropdown-link">
            不限
            <i class="el-icon-arrow-down el-icon--right"></i>
          </span>
          <el-dropdown-menu slot="dropdown" class="dropdown" split-button="true">
            <el-dropdown-item>黄金糕</el-dropdown-item>
            <el-dropdown-item>狮子头</el-dropdown-item>
            <el-dropdown-item>螺蛳粉</el-dropdown-item>
            <el-dropdown-item>双皮奶</el-dropdown-item>
            <el-dropdown-item>蚵仔煎</el-dropdown-item>
          </el-dropdown-menu>
        </el-dropdown>
      </el-col>
    </el-row>
    <el-row type="flex" class="row-bg" justify="space-between">
      <el-col :span="8">
        <div class="grid-content bg-purple">
          <nuxt-link to="#">
            <img
              src="http://qcloud.dpfile.com/pc/wQ2ejOSmhB4OoEq1LYPHp_2RhNciyW026YiQFr_EEZsQrI_ZB6abZPwgKDOmI03U3dWYFEM5hoslgWXTJDjSPA.jpg"
              alt
            />
          </nuxt-link>
        </div>
      </el-col>
      <el-col :span="10">
        <div class="grid-content bg-purple-light">
          <h4 class="hotel-cn-name">
            <nuxt-link to="#">好来阁商务宾馆</nuxt-link>
          </h4>
          <span>hao lai ge shang wu hotel</span>
          <span class="namecolor">
            <i class="iconfont iconhuangguan"></i>
            <i class="iconfont iconhuangguan"></i>
            <i class="iconfont iconhuangguan"></i>
          </span>
          <span>经济型</span>
          <el-row type="flex" justify="space-between">
            <el-col>
              <el-rate v-model="value1" :colors="colors" disabled></el-rate>
            </el-col>
            <el-col>
              <span class="namecolor">3.5分</span>
            </el-col>
            <el-col>
              <span class="namecolor">9</span>条评价
            </el-col>
            <el-col>
              <span class="namecolor">37</span>篇游记
            </el-col>
          </el-row>
          <el-row class="location-row">
            <i class="iconfont iconlocation"></i>
            <span>位于: 高淳县淳溪镇镇兴路118号(高淳县委党校对面)</span>
          </el-row>
        </div>
      </el-col>
      <el-col :span="6">
        <div class="grid-content bg-purple">
          <el-table show-header:false :data="tableData" style="width: 100%">
            <el-table-column prop="date"></el-table-column>
            <el-table-column v-for="(item,index) in tableData" :key="index">
              <span class="namecolor">{{item.price}}</span>起
              <i class="el-icon-arrow-right"></i>
            </el-table-column>
          </el-table>
        </div>
      </el-col>
    </el-row>
    <el-row type="flex" justify="end"> 
      <el-pagination
      :current-page="pageIndex"
      layout="total, prev, pager, next"
      :total="total"
    ></el-pagination>
    </el-row>
  </div>
</template>
<script>
export default {
  data() {
    return {
      value2: 0,
      value1: 3.5,
      colors: ["#99A9BF", "#F7BA2A", "#FF9900"],
      tableData: [
        {
          date: "携程",
          price: "￥ 159"
        }
      ],
      total:100,
      pageIndex:1
    };
  },
  methods: {
    formatTooltip(val) {
      return val / 100;
    }
  }
};
</script>
<style lang='less' scoped>
.hotel-cn-name {
  font-weight: 400;
  font-size: x-large;
}
.el-row {
  margin-bottom: 20px;
  &:last-child {
    margin-bottom: 0;
  }
}
.location-row {
  font-size: 14px;
  color: #666;
}
.namecolor {
  color: #f90;
}
.el-col {
  border-radius: 4px;
  box-sizing: border-box;
  img {
    width: 100%;
    height: 100%;
  }
}
.bg-purple {
  background: #fff;
}
.bg-purple-light {
  background: #fff;
}
.grid-content {
  border-radius: 4px;
  min-height: 36px;
  height: 214px;
  margin: 10px;
}
.row-bg {
  height: 214px;
  padding: 10px 0;
  background-color: #fff;
}
.elright {
  padding: 0 20px 0 10px;
}
.price {
  padding: 0 200px 0 10px;
}
.el-slider {
  padding-left: 20px;
  width: 260px;
}
.screen01 {
  padding: 10px;
  margin-top: 10px;
  border: 1px solid #ccc;
}
.elleft {
  border-left: 1px solid #ccc;
  padding-left: 20px;
}
.el-dropdown-link {
  cursor: pointer;
  color: #8492a6;
}
.el-icon-arrow-down {
  font-size: 12px;
  margin-left: 90px;
}
.demonstration {
  color: #8492a6;
  font-size: 14px;
  margin: 0px;
}
</style>