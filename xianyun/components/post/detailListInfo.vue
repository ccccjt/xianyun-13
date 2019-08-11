<template>
    <div>
        <abc v-for="(item, index) in data" :key="index"
        :style="`height:${height*(index+1)}px;width:${width + (index+100)}px`">
        </abc>
    </div>
</template>

<script>
export default {
    name:'abc',
    props: ["data"],
    data(){
        return{
            height:"100",
            width:"100"
        }
    }
}
</script>

<style scoped>
div{
    border:2px solid #764;
    padding: 50px;
    margin-bottom: 50px;
}
</style>
