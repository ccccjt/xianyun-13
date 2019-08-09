<template>
    <div class="comment-list">

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
