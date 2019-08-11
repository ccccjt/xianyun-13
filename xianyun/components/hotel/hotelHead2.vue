<template>
  <div class="container">
    <header class="header">
      <!-- 头部搜索部分 -->
      <div class="search">
        <el-form :inline="true"
                 :model="formInline"
                 class="demo-form-inline">
          <el-select v-model="formInline.destCity"
                    multiple
                    filterable
                    remote
                    reserve-keyword
                    value="南京"
                    placeholder="目的地"
                    :remote-method="remoteMethod"
                    :loading="loading">
            <el-option v-for="item in options"
                       :key="item.value"
                       :label="item.label"
                       :value="item.value">
            </el-option>
          </el-select>
          <el-date-picker v-model="formInline.date"
                          type="daterange"
                          range-separator="-"
                          start-placeholder="开始日期"
                          end-placeholder="结束日期">
          </el-date-picker>
          <el-form-item class="people">
            <el-select v-model="formInline.region"
                       placeholder="人数未定">
              <el-option label="区域一"
                         value="tianjing"
                         style="border-bottom: 1px solid #aaa;">
              </el-option>
              <el-option label="区域二"
                         value="beijing"></el-option>
            </el-select>
            <div class="icon el-icon-user"></div>
          </el-form-item>

          <el-form-item>
            <el-button type="primary"
                       @click="onSubmit">查询价格</el-button>
          </el-form-item>
        </el-form>
      </div>

      <!-- 描述部分 -->
      <div class="descript">
        <el-row>
          <el-col :span="14">
            <div class="grid-content bg-purple">
              <el-form :label-position="labelPosition"
                       label-width="60px"
                       :model="formLabelAlign">

                <el-form-item label="区域:">
                  <div class="details">
                    <nuxt-link to="#">全部</nuxt-link>
                    <a v-for="(item, index) in showdetailList"
                       :key="index">
                      {{item}}
                    </a>
                    <div v-if="detailList.length > 12"
                         v-on:click="changeFoldState">
                      <p><i class="el-icon-arrow-down"
                           v-show="brandFold"></i><i class="el-icon-arrow-up"
                           v-show="!brandFold"></i>{{brandFold?'等43个区域展开':'收起'}}</p>
                    </div>
                  </div>

                </el-form-item>
                <el-form-item label="攻略:">
                  <p>北京，你想要的都能在这找到。博古通今，兼容并蓄，天下一城，如是帝都。 景点以故宫为中心向四处辐射；地铁便宜通畅，而且覆盖绝大多数景点。 由于早上有天安门升旗仪式，所以大多数人选择在天安门附近住宿。</p>
                </el-form-item>
                <el-form-item label="均价　 :">
                  <el-tooltip content="等级均价由平日价格计算得出，节假日价格会有上浮。"
                              placement="top-start">
                    <i class="el-icon-question"></i>
                  </el-tooltip>
                  <el-tooltip content="等级评定是针对房价，设施和服务等各方面水平的综合评估。"
                              placement="bottom-start"
                              :visible-arrow="false">
                    <span>
                      <i class="iconfont icon-icon_huangguan"></i>
                      <i class="iconfont icon-icon_huangguan"></i>
                      <i class="iconfont icon-icon_huangguan"></i>
                      &nbsp;&nbsp;¥332
                    </span>
                  </el-tooltip>
                  <el-tooltip content="等级评定是针对房价，设施和服务等各方面水平的综合评估。"
                              placement="bottom-start"
                              :visible-arrow="false">
                    <span>
                      <i class="iconfont icon-icon_huangguan"></i>
                      <i class="iconfont icon-icon_huangguan"></i>
                      <i class="iconfont icon-icon_huangguan"></i>
                      <i class="iconfont icon-icon_huangguan"></i>
                      &nbsp;&nbsp;¥521
                    </span>
                  </el-tooltip>
                  <el-tooltip content="等级评定是针对房价，设施和服务等各方面水平的综合评估。"
                              placement="bottom-start"
                              :visible-arrow="false">
                    <span>
                      <i class="iconfont icon-icon_huangguan"></i>
                      <i class="iconfont icon-icon_huangguan"></i>
                      <i class="iconfont icon-icon_huangguan"></i>
                      <i class="iconfont icon-icon_huangguan"></i>
                      <i class="iconfont icon-icon_huangguan"></i>
                      &nbsp;&nbsp;¥768
                    </span>
                  </el-tooltip>
                </el-form-item>
              </el-form>
            </div>
          </el-col>
          <el-col :span="10">
            <div id="container"></div>
          </el-col>
        </el-row>
      </div>

    </header>
    <script type="text/javascript"
            src="https://webapi.amap.com/maps?v=1.4.15&key=a34ddeb57016dd95d03a44033ffd87e0"></script>
    <script>
    var map = new AMap.Map('container');
    var map = new AMap.Map('container', {
      zoom: 10, //级别
      center: [118.7544, 32.1139], //中心点坐标
      viewMode: '3D' //使用3D视图
    })
    // 创建一个 Marker 实例：
    var marker = new AMap.Marker({
      position: new AMap.LngLat(118.7944, 32.0639), // 经纬度对象，也可以是经纬度构成的一维数组[118.7544, 32.1139]
      title: '南京'
    });
    // 将创建的点标记添加到已有的地图实例：
    map.add(marker);
    </script>
  </div>

</template>

<script>
export default {
  data () {
    return {
      // 人数选择框
      formInline: {
        destCity: '',
        date: '',
        region: '',
      },
      // 描述部分
      labelPosition: 'left',
      value: 0,
      // 选择日期
      value1: '',
      loading: false,
      // 目的地的内容
      options: {},
      formLabelAlign: {},
      brandFold: true,
      detailList: ["镇兴路沿线", "视觉艺术学院", "大成名店", "南京西站", "铜山镇", "大桥南路", "宝塔路沿线", "宝塔路/万辰苏果", "珠江路沿线", "华侨城", "江浦", "东屏镇", "南京南站/明发", "北岭路沿线", "苜蓿园", "弘阳广场/新一城", "新街口地区", "紫金山/孝陵卫", "火车站/玄武湖", "东坝镇", "禄口机场", "奥体中心", "雨润大街", "新模范马路", "将军山", "国际慢城", "云鼎时尚街区", "百家湖", "湖南路", "竹山路沿线", "南大/南师大", "江宁滨江开发区", "湖熟镇", "南大和园", "君临紫金商业街", "大西门", "建邺万达", "江宁科学园", "顾家欧亚达", "高淳老街", "谷里", "汤山镇", "雄州"],
    }
  },
  methods: {
    queryDepartSearch () {

    },
    // 查询价格按钮点击触发
    onSubmit () {
      console.log(this.form);
    },
    remoteMethod () {

    },
    clickEvent () {

    },
    changeFoldState () {
      this.brandFold = !this.brandFold
    }
  },
  computed: {
    showdetailList: {
      get: function () {
        if (this.brandFold) {
          if (this.detailList.length < 13) {
            return this.detailList
          }
          let newArr = []
          for (var i = 0; i < 12; i++) {
            let item = this.detailList[i]
            newArr.push(item)
          }
          return newArr
        }
        return this.detailList
      },
      set: function (val) {
        this.showdetailList = val
      }
    }
  }
}
</script>

<style scoped lang="less">
.icon {
  width: 1em;
  height: 1em;
  vertical-align: -0.15em;
  fill: currentColor;
  overflow: hidden;
}
.search {
  height: 40px;
  line-height: 40px;
  padding: 20px 0;
}

.people {
  position: relative;
  .el-scrollbar {
    padding: 0 10px;
    // margin: 0 10px;
  }
  .icon {
    position: absolute;
    height: 40px;
    line-height: 40px;
    font-size: 20px;
    width: 25px;
    top: 0;
    right: 5px;
    color: #c0c4cc;
  }
}
.descript {
  font-size: 14px;
  color: #666;
  .el-link {
    margin: 0 5px;
    height: 18px;
    line-height: 18px;
    overflow: hidden;
  }
  .details {
    display: flex;
    flex-wrap: wrap;
    // height: 42px;
    // overflow: hidden;
    a {
      margin-right: 15px;
      &:hover {
        text-decoration: underline;
        color: blue;
      }
    }
    p {
      color: #ccc;
      cursor: pointer;
      i {
        color: #ffa963;
      }
    }
  }
  span {
    margin-right: 30px;
  }
}
.icon-icon_huangguan {
  color: #ff9900;
}
#container {
  width: 410px;
  height: 280px;
  margin-left: 10px;
}
</style>
<style lang="less">
.container {
  .el-select .el-input .el-select__caret {
    display: none;
  }
  .el-form-item__content {
    line-height: 20px;
    position: relative;
    .el-icon-question {
      position: absolute;
      left: -30px;
      color: #ccc;
    }
  }
}
.el-form-item__label {
  line-height: unset;
  padding-right: 0;
}
</style>

