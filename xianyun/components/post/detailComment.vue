<template>
  <div class="comment">
    <h3>评论</h3>

    <el-tag
    closable
    type="info"
    class="replyTag"
    v-if="reply.nickname"
    @close="closeReply">
    回复 @{{reply.nickname}}
    </el-tag>

    <el-input
      type="textarea"
      :rows="2"
      placeholder="说点什么吧"
      v-model="form.content"
      style="margin-bottom:10px;"
      resize="none"
    ></el-input>

    <el-upload
      action="http://127.0.0.1:1337/upload"
      list-type="picture-card"
      :on-preview="handlePictureCardPreview"
      :on-remove="handleRemove"
      :on-success="handleSuccess"
      name="files"
      ref="upload"
    >
      <i class="el-icon-plus"></i>
    </el-upload>
    <el-dialog :visible.sync="dialogVisible">
      <img width="100%" :src="dialogImageUrl" alt />
    </el-dialog>

    <el-button type="primary" class="btnsubmit" size="small" @click="submitComment">提交</el-button>


    <div class="noComment" v-if="!total">
        目前还没有评论，快来评论吧！
    </div>


    <div class="comment-list">
      <div class="listitem" v-for="(item, index) in list" :key="index">
        <div class="list-info">
          <img :src="$axios.defaults.baseURL + item.account.defaultAvatar" />
          {{item.account.nickname}}
          <i>{{item.created_at | timeFormat}}</i>
          <span>{{item.level}}</span>
        </div>
        <div class="list-content">
          <DetailListInfo v-if="item.parent" :data="item.parent" @preview="handlePic" @reply="handleReply"/>

          <div class="cmt-new">
            <p class="cmt-message">{{item.content}}</p>
            <el-row type="flex">
              <div class="cmt-pic" v-for="(pic, picIndex) in item.pics" :key="picIndex">
                <img :src="$axios.defaults.baseURL + pic.url" @click="handlePic(pic)"/>
              </div>
            </el-row>
            <div class="cmt-ctrl">
              <a href="javascript:;" @click="handleReply(item)">回复</a>
            </div>
          </div>
        </div>
      </div>
      <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="pageindex"
        :page-sizes="[1, 2, 3, 4]"
        :page-size="pagesize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total"
      ></el-pagination>
    </div>
  </div>
</template>

<script>
import DetailListInfo from "@/components/post/detailListInfo.vue";

import moment from "moment";

export default {
  data() {
    return {
      form:{
        content:'',
        pics:[]
      },
      dialogImageUrl: "",
      dialogVisible: false,

      pageindex: 1,
      pagesize: 4,
      total: 0,

      list: [],

      //用户存放被回复信息
      reply:{}

    };
  },
  props: {
    id: {
      type: String,
      default: ""
    }
  },

  components: {
    DetailListInfo
  },

  methods: {
    closeReply(){
      this.reply = {};
      this.form.follow = ""
    },
    handleReply(comment){
      this.form.follow = comment.id
      this.reply = comment.account
    },
    handlePic(pic){
      if(pic.response){
          pic = file.response[0]
      }
      this.dialogImageUrl = file.url;
      this.dialogVisible = true;
    },
    handleRemove(file, fileList) {
      // console.log(file.response[0].id,"被移除的id");
      this.form.pics.forEach((item, index) => {
        if (item.id === file.response[0].id) {
          this.form.pics.splice(index, 1);
        }
      });
      // console.log(this.pics,"移除后的数组");
    },
    handlePictureCardPreview(file) {
      this.dialogImageUrl = file.url;
      this.dialogVisible = true;
    },
    handleSuccess(response, file, fileList) {
      // console.log(response);
      this.form.pics.push(response[0]);
      // console.log(this.form.pics,"数组");
    },

    handleSizeChange(val) {
      this.pagesize = val;
      this.pageindex = 1;
      this.getCommentsData();
    },
    handleCurrentChange(val) {
      this.pageindex = val;
      this.getCommentsData();
    },

    submitComment() {
      this.$axios({
        url: "/comments",
        method: "POST",
        data: {
          ...this.form,
          post: this.id
        },
        headers: {
          Authorization: `Bearer ${this.$store.state.user.userInfo.token}`
          // ContentType:'application/json;charset=UTF-8'
        }
      }).then(res => {
        // console.log(res);
        if (res.data.status == 0) {
          this.form={
            content:'',
            pics:[]
          }
          this.closeReply()
          this.$refs.upload.clearFiles();
          this.getCommentsData();
          this.$message.success(res.data.message);
        }
      });
    },

    getCommentsData() {
      setTimeout(() => {
        this.$axios({
          url: "/posts/comments",
          params: {
            post: this.id,
            _limit: this.pagesize,
            _start: this.pageindex - 1
          }
        }).then(res => {
          console.log(res);
          this.list = res.data.data;
          this.total = res.data.total;
        });
      }, 1);
    }
  },

  mounted() {
    this.getCommentsData();
    
  },
  updated () {
    this.$emit("sendTotal",this.total)
  },
  watch: {
    $route(){
      this.getCommentsData()
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
.comment {
  position: relative;
}
.comment h3 {
  font-weight: normal;
  padding-bottom: 20px;
}
.btnsubmit {
  height: 25px;
  width: 52px;
  position: absolute;
  right: 0;
  top: 135px;
}
.noComment{
  margin-top: 30px;
  color:#999;
  font-size: 18px;
  text-align: center;
  padding:30px 0;
  border:1px #ddd dashed;
}
.comment-list {
  margin-top: 30px;
  border: 1px solid #ccc;
}
.listitem {
  border-bottom: 1px #ddd dashed;
  padding: 20px 20px 5px;
}
.list-info {
  margin-bottom: 10px;
  font-size: 12px;
  color: #666;
  * {
    vertical-align: top;
  }

  img {
    width: 16px;
    height: 16px;
    border-radius: 50%;
  }

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
/*  这里和comment floor不一样，鼠标在最新内容上才hover */
.cmt-new:hover {
  .cmt-ctrl {
    * {
      display: inline;
    }
  }
}
</style>