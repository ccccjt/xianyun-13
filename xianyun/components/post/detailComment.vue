<template>
  <div class="comment">
    <h3>评论</h3>
    <el-input
      type="textarea"
      :rows="2"
      placeholder="说点什么吧"
      v-model="textarea"
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

    <div class="comment-list">
      <div class="listInfo">
        <DetailListInfo v-for="(item, index) in list" :key="index" :data="item"/>
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
import DetailListInfo from '@/components/post/detailListInfo.vue';
export default {
  data() {
    return {
      textarea: "",
      dialogImageUrl: "",
      dialogVisible: false,

      pageindex: 1,
      pagesize: 4,
      total: 100,

      pics: [],

      list: [
                {
                    title: "衣服",
                    children: [
                        { title: "男装" , children: [
                            { title: "老男人" },
                            { title: "正规男装" },
                        ]} ,
                        { title: "女装", children: [
                            { title:  "正规女装"},
                            { title:  "正规透视装"},
                        ] },
                        { title: "童装" }
                    ]
                },
                 {
                    title: "电器", children: [
                         { title: "正规按摩椅" },
                         { title: "正规电饭煲" },
                    ]
                }
            ]
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
    handleRemove(file, fileList) {
      // console.log(file.response[0].id,"被移除的id");
      this.pics.forEach( (item,index) => {
          if(item.id === file.response[0].id){
            this.pics.splice(index,1)
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
      this.pics.push(response[0])
      // console.log(this.pics,"数组");
    },

    handleSizeChange(val) {
      this.pagesize = val
      this.pageindex = 1
      this.getCommentsData()
    },
    handleCurrentChange(val) {
      this.pageindex = val
      this.getCommentsData()
    },

    submitComment() {
      this.$axios({
        url:'/comments',
        method:'POST',
        data:{
          content:this.textarea,
          pics:this.pics,
          post:this.id
        },
        headers:{
          Authorization: `Bearer ${this.$store.state.user.userInfo.token}`,
          // ContentType:'application/json;charset=UTF-8'
        },
      }).then(res=>{
        // console.log(res);
        if(res.data.status == 0){
          this.textarea = ""
          this.pics = []
          this.$refs.upload.clearFiles();
          this.getCommentsData()
          this.$message.success(res.data.message)
        }
      })
    },

    getCommentsData(){
        setTimeout(()=>{
            this.$axios({
                url:'/posts/comments',
                params:{
                    post:this.id,
                    _limit:this.pagesize,
                    _start:this.pageindex - 1
                }
            }).then(res=>{
                console.log(res);
                this.total = res.data.total
            })
        },1)
    }
  },

  mounted(){
    this.getCommentsData()
  }
};
</script>

<style scoped>
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
  top: 110px;
}
.comment-list {
  margin-top: 30px;
}
.listInfo {
  height: 274px;
  border: 1px solid #ddd;
  margin-bottom: 15px;
}
</style>
