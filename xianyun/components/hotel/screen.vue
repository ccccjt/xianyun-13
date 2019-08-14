<template>
  <div class>
    <el-row class="block-col-2 screen01" type="flex">
      <el-col :span="8" class="elright" justify="center">
        <span class="demonstration price">价格</span>
        <span class="demonstration">0-{{value2}}</span>
        <el-slider v-model="value2" :format-tooltip="formatTooltip"></el-slider>
      </el-col>
      <el-col :span="4" class="elleft">
        <span class="demonstration">住宿等级</span>
        <el-select v-model="hotellevel" multiple placeholder="不限" collapse-tags>
          <el-option v-for="item in levels" :key="item.value" :label="item.name" :value="item.id"></el-option>
        </el-select>
      </el-col>
      <el-col :span="4" class="elleft" justify="center">
        <span class="demonstration">住宿类型</span>
        <el-select v-model="hoteltype" multiple placeholder="不限" collapse-tags>
          <el-option v-for="item in types" :key="item.value" :label="item.name" :value="item.id"></el-option>
        </el-select>
      </el-col>
      <el-col :span="4" class="elleft">
        <span class="demonstration">酒店设施</span>
        <el-select v-model="hotelasset" multiple placeholder="不限" collapse-tags>
          <el-option v-for="item in assets" :key="item.value" :label="item.name" :value="item.id"></el-option>
        </el-select>
      </el-col>
      <el-col :span="4" class="elleft">
        <span class="demonstration">酒店品牌</span>
        <el-select v-model="hotelbrand" multiple placeholder="不限" collapse-tags>
          <el-option v-for="item in brands" :key="item.value" :label="item.name" :value="item.id"></el-option>
        </el-select>
      </el-col>
    </el-row>
    <el-row
      type="flex"
      class="row-bg"
      justify="space-between"
      v-for="(item,index) in hotelList"
      :key="index"
    >
      <el-col :span="8">
        <div class="grid-content bg-purple">
          <nuxt-link :to="`hotel/${item.id}.html`">
            <img :src="`${item.photos}`" alt />
          </nuxt-link>
        </div>
      </el-col>
      <el-col :span="10">
        <div class="grid-content bg-purple-light">
          <h4 class="hotel-cn-name">
            <nuxt-link :to="`hotel/${item.id}.html`">{{item.name}}</nuxt-link>
          </h4>
          <span>{{item.alias}}</span>
          <span class="namecolor">
            <i class="iconfont iconhuangguan"></i>
            <i class="iconfont iconhuangguan"></i>
            <i class="iconfont iconhuangguan"></i>
          </span>
          <span>{{item.hoteltype.name}}</span>
          <el-row type="flex" justify="space-between">
            <el-col>
              <el-rate :score-template="`{${item.stars}}`" :colors="colors" disabled></el-rate>
            </el-col>
            <el-col>
              <span class="namecolor">{{item.stars}}分</span>
            </el-col>
            <el-col>
              <span class="namecolor">{{item.all_remarks}}</span>条评价
            </el-col>
            <el-col>
              <span class="namecolor">{{item.num_collected}}</span>篇游记
            </el-col>
          </el-row>
          <el-row class="location-row">
            <i class="iconfont iconlocation"></i>
            <span>位于: {{item.address}}</span>
          </el-row>
        </div>
      </el-col>
      <el-col :span="6">
        <div class="grid-content bg-purple">
          <el-table show-header:false :data="item.products" style="width: 100%">
            <el-table-column prop="name"></el-table-column>
            <el-table-column prop="price">
              <template slot-scope="scope">
                <span class="namecolor">￥{{ scope.row.price }}</span>起
                <i class="el-icon-arrow-right"></i>
              </template>
            </el-table-column>
          </el-table>
        </div>
      </el-col>
    </el-row>
    <el-row type="flex" justify="end">
      <el-pagination :current-page="pageIndex" layout="total, prev, pager, next" :total="total">

      </el-pagination>
    </el-row>
  </div>
</template>
<script>
export default {
  data() {
    return {
      value2: 0,
      colors: ["#99A9BF", "#F7BA2A", "#FF9900"],
      hotelList: [],
      hoteltype: [],
      hotellevel: [],
      hotelasset: [],
      hotelbrand: [],
      levels: [], // 酒店等级
      types: [], // 酒店类型
      assets: [], // 酒店设施
      brands: [], // 酒店品牌
      total: 100,
      pageIndex: 1,
      pageSize:2,
    };
  },
  methods: {
    formatTooltip(val) {
      this.value2=val
      return val / 0.025;
    },
    handleHotellevel(value) {
      console.log(value);
      this.hotellevel = this.levels[value - 1].name;
    },
    handleHoteltype(value) {
      console.log(value);
      this.hoteltype = this.types[value - 1].name;
    },
    handleHotelasset(value) {
      console.log(value);
      this.hotelasset = this.assets[value - 1].name;
    },
    handleHotelbrand(value) {
      console.log(value);
      this.hotelbrand = this.brands[value - 1].name;
    }
  },
  watch: {
    "value2"(){
      this.$axios({
      url: "/hotels",
      params: { city: 74 }
    }).then(res => {
      console.log(res);
      this.total=res.data.data.length;
      this.hotelList = res.data.data;
    });
    }
  },
  mounted() {
    this.$axios({
      url: "/hotels/options"
    }).then(res => {
      console.log(res);
      this.assets = res.data.data.assets;
      this.levels = res.data.data.levels;
      this.types = res.data.data.types;
      this.brands = res.data.data.brands;
    });
    this.$axios({
      url: "/hotels",
      params: { city: 74 }
    }).then(res => {
      console.log(res);
      this.total=res.data.data.length;
      this.hotelList = res.data.data;
    });
    this.$axios({
      url:'/posts/comments?id=33'
    }).then(res=>{
      console.log(res);
    })
  }
};
</script>
<style lang='less' scoped>

/deep/ .el-input__inner {
  border: none;
  padding: 0 30px 0 0;
}
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
  height: 250px;
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