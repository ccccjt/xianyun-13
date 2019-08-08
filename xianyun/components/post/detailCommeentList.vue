<template>
    <div class="comment-list">
        <div class="listInfo">

        </div>
        <el-pagination
        @size-change="handleSizeChange"
        @current-change="handleCurrentChange"
        :current-page="pageindex"
        :page-sizes="[4, 6, 8, 10]"
        :page-size="pagesize"
        layout="total, sizes, prev, pager, next, jumper"
        :total="total">
        </el-pagination>
    </div>
</template>

<script>
export default {
    data(){
        return{
            pageindex:1,
            pagesize:4,
            total:100
        }
    },

    methods: {
        handleSizeChange(val){
            console.log(`每页 ${val} 条`);
        },
        handleCurrentChange(val) {
            console.log(`当前页: ${val}`);
        }
    }
}
</script>

<style scoped>
.comment-list{
margin-top: 30px;
}
.listInfo{
    height:274px ;
    border: 1px solid #ddd;
    margin-bottom: 15px;
}
</style>
