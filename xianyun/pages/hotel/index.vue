<template>
    <div class="w">
<!-- 酒店页面地区查询组件 -->
    <div>


        </div>
      
<!-- 酒店筛选组件 -->
        <div class="screen"> 
            <Screen/>
            
        </div>
        
    </div>
</template>

<script>
import Screen from '@/components/hotel/screen'
export default {
  components: {
      Screen
  }
}
</script>

<style>
    #container {width:500px; height: 500px; }  
.screen{
    margin-top: 50px;
    height: 80px;
}
.w{
        width: 1000px;
        margin: 0 auto;
        height: 1000px;
}
</style>

