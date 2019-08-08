<template>
    <div class="comment">
        <h3>评论</h3>
        <el-input
        type="textarea"
        :rows="2"
        placeholder="说点什么吧"
        v-model="textarea"
        style="margin-bottom:10px;">
        </el-input>
        <el-upload
        action="https://jsonplaceholder.typicode.com/posts/"
        list-type="picture-card"
        :on-preview="handlePictureCardPreview"
        :on-remove="handleRemove">
        <i class="el-icon-plus"></i>
        </el-upload>
        <el-dialog :visible.sync="dialogVisible">
        <img width="100%" :src="dialogImageUrl" alt="">
        </el-dialog>
        <el-button type="primary" class="btnsubmit" size="small">提交</el-button>
    </div>
</template>

<script>
export default {
    data(){
        return{
            textarea:'' ,
            dialogImageUrl: '',
            dialogVisible: false,
        }
    },

    methods: {
        handleRemove(file, fileList) {
            console.log(file, fileList);
        },
        handlePictureCardPreview(file) {
            this.dialogImageUrl = file.url;
            this.dialogVisible = true;
        }
    }
}
</script>

<style scoped>
.comment{
    position: relative;

}
.comment h3{
    font-weight: normal;
    padding-bottom: 20px;
}
.btnsubmit{
    height: 25px;
    width: 52px;
    position: absolute;
    right: 0;
    top: 110px;
}
</style>
