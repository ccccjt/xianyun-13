<template>
  <div class="listinfo">
    <abc
      v-if="data.parent"
      :data="data.parent"
      @preview="handlePic"
      @reply="handleReply"
    ></abc>
    <div class="cmt-content">
      <div class="cmt-info">
        {{data.account.nickname}}
        <i>{{data.created_at | timeFormat}}</i>
        <span>{{data.level}}</span>
      </div>
      <p class="cmt-message">{{data.content}}</p>
      <el-row type="flex">
        <div class="cmt-pic" v-for="(pic, picIndex) in data.pics" :key="picIndex">
          <img :src="$axios.defaults.baseURL +pic.url" @click="handlePic(pic)" />
        </div>
      </el-row>
      <div class="cmt-ctrl">
        <a href="javascript:;" @click="handleReply(item)">回复</a>
      </div>
    </div>

    <!-- <abc
      v-if="data.parent"
      :data="data.parent"
      @preview="handlePic"
      @reply="handleReply"
    ></abc> -->
  </div>
</template>

<script>
import moment from "moment";

export default {
  name: "abc",

  data() {
    return {};
  },

  props: {
    data: {
      type: Object
    }
  },

  methods: {
    handlePic(file) {
      this.$emit("preview", file);
    },

    handleReply() {
      this.$emit("reply", this.comment);
    }
  },

  filters: {
    timeFormat(value) {
      return moment(value).format("YYYY-MM-DD h:mm");
    }
  }
};
</script>

<style scoped lang="less">
.listinfo {
  background: #f9f9f9;
  border: 1px #ddd solid;
  padding: 2px;

  .cmt-content {
    padding: 5px 10px 0;
    .cmt-info {
      font-size: 12px;
      color: #666;
      i {
        color: #999;
      }
      span {
        float: right;
      }
    }
    .cmt-message {
      margin-top: 10px;
    }

    .cmt-pic {
      width: 80px;
      height: 80px;
      border-radius: 6px;
      overflow: hidden;
      margin-right: 5px;
      margin-top: 10px;
      padding: 5px;
      border: 1px #ddd dashed;

      img {
        display: block;
        width: 100%;
        height: 100%;
        object-fit: cover;
        cursor: pointer;
      }
    }

    &:hover {
      .cmt-ctrl {
        * {
          display: inline;
        }
      }
    }

    .cmt-ctrl {
      height: 20px;
      line-height: 20px;
      font-size: 12px;
      color: #1e50a2;
      text-align: right;

      * {
        display: none;
      }

      a:hover {
        text-decoration: underline;
      }
    }
  }
}
</style>
