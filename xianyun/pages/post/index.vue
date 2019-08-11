<template>
  <div class="container">
    <el-row type="flex" justify="space-between">
      <!-- 左侧菜单栏 -->

      <!-- 右侧内容 -->
      <div class="post-wrapper">
        <div class="search-wrapper">
          <el-row type="flex" justify="space-between" align="middle" class="search-bar">
            <input
              type="text"
              placeholder="请输入想去的地方，比如：'广州'"
              v-model="city"
              @keyup.enter="handleSearch"
            />
            <el-button type="primary" icon="el-icon-search">搜索</el-button>
          </el-row>
          <div class="search-recommend">
            推荐：
            <span v-for="(item, index) in ['奥尔良 ','拉萨 ',' 西双版纳']" :key="index">{{item}}</span>
          </div>
        </div>
        <el-row type="flex" justify="space-between" align="middle" class="post-title">
          <h4>推荐攻略</h4>
          <el-button type="primary" size="small" icon="el-icon-edit">写游记</el-button>
        </el-row>

        <div class="post-list">
          <postRight v-for="(item, index) in posts" :key="index" :data="item"></postRight>
        </div>

        <el-row type="flex" justify="center" style="margin-top:10px;">
          <el-pagination
            @size-change="handleSizeChange"
            @current-change="handleCurrentChange"
            :current-page="Math.floor(start / limit) + 1"
            :page-sizes="[4, 8, 10, 15]"
            :page-size="limit"
            layout="total, sizes, prev, pager, next, jumper"
            :total="total"
          ></el-pagination>
        </el-row>
      </div>
    </el-row>
  </div>
</template>

<script>
import postRight from "@/components/post/postRight";

export default {
  data() {
    return {
      posts: [],
      cities: [],

      city: this.$route.query.city || "",
      start: 0,
      limit: 4,
      total: 0
    };
  },
  components: {
    postRight
  },
  watch: {
    $route(value) {
      const { city } = value.query;
      this.city = city;
      this.start = 0;
      this.getPost();
    }
  },
  methods: {
    getPost() {
      const { api } = this.$store.state;
      const params = {
        _start: this.start,
        _limit: this.limit
      };

      if (this.city) {
        params.city = this.city;
      }

      this.$axios({
        url: "http://157.122.54.189:9095/posts",
        params
      }).then(res => {
        const { data, total } = res.data;
        this.posts = data;
        this.total = total;
      });
    },

    handleSizeChange(value) {
      this.limit = value;
      this.start = 0;
      this.getPost();
    },

    handleCurrentChange(value) {
      this.start = this.limit * (value - 1);
      this.getPost();
    },

    // 搜索
    handleSearch(value) {
      if (typeof value == "string") {
        this.city = value;
      }

      this.start = 0;
      this.getPost();
    },

    // 获取城市
    getCities() {
      const { api } = this.$store.state;

      this.$axios({
        url: "http:127.0.0.1:1337/posts/cities"
      }).then(res => {
        const { data } = res.data;
        this.cities = data;
      });
    }
  },

  mounted() {
    this.getPost();
    this.getCities();
  }
};
</script>

<style scoped lang="less">
.container {
  width: 1000px;
  margin: 0 auto;
  padding: 20px 0;
}

/* 菜单栏 */
.menus-wrapper {
  position: relative;
  width: 260px;
  z-index: 2;

  .menus {
    width: 260px;
    border: 1px #ddd solid;
    border-right: none;
    border-bottom: none;
    box-shadow: 0 0 1px #f5f5f5;
    z-index: 2;
  }

  .menu-item {
    height: 40px;
    line-height: 40px;
    border-bottom: 1px #ddd solid;
    border-right: 1px #ddd solid;
    padding: 0 20px;
    font-size: 14px;
    position: relative;
    z-index: 2;

    &:after {
      display: block;
      content: "";
      width: 10px;
      height: 10px;
      border-right: 1px #999 solid;
      border-top: 1px #999 solid;
      transform: rotate(45deg);
      position: absolute;
      right: 20px;
      top: 15px;
    }

    &.active {
      border-right-color: #fff;
      color: orange;

      &:after {
        border-right-color: orange;
        border-top-color: orange;
      }
    }
  }

  .sub-menus {
    position: absolute;
    left: 260px;
    top: 0;
    width: 350px;
    padding: 10px 20px;
    box-sizing: border-box;
    background: #fff;
    border: 1px #ddd solid;

    ul li {
      font-size: 14px;
      line-height: 1.5;

      * {
        vertical-align: middle;
      }

      i {
        color: orange;
        font-size: 24px;
        font-style: italic;
      }

      strong {
        margin: 0 10px;
        color: orange;
        font-weight: normal;
        &:hover {
          text-decoration: underline;
        }
      }

      span {
        color: #999;
        &:hover {
          text-decoration: underline;
        }
      }
    }
  }
}

.post-wrapper {
  width: 700px;
}

.search-wrapper {
  .search-bar {
    width: 100%;
    box-sizing: border-box;
    height: 40px;
    line-height: 40px;
    border: 3px orange solid;
    border-right: 0;

    input {
      flex: 1;
      padding: 0 20px;
      line-height: 40px;
      outline: none;
      border: none;
      background: none;
    }

    i {
      font-size: 24px;
      color: orange;
      font-weight: bold;
      margin-right: 10px;
    }
  }

  .search-recommend {
    padding: 10px 0;
    font-size: 12px;

    span {
      margin-right: 5px;
      &:hover {
        color: orange;
        text-decoration: overline;
        cursor: pointer;
      }
    }
  }
}

/* 标题 */
.post-title {
  padding-bottom: 10px;
  border-bottom: 1px #eee solid;
  position: relative;

  h4 {
    font-weight: normal;
    font-size: 18px;
    color: orange;

    &:after {
      display: block;
      content: "";
      width: 72px;
      height: 2px;
      background: orange;
      position: absolute;
      bottom: 0;
      left: 0;
    }
  }
}
</style>
