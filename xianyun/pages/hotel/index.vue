<template>
  <div class="w">
    <!-- 酒店页面地区查询组件 -->
    <div class="title">
      <el-breadcrumb separator-class="el-icon-arrow-right">
        <el-breadcrumb-item :to="{ path: '/hotel' }">酒店</el-breadcrumb-item>
        <el-breadcrumb-item>南京市酒店预订</el-breadcrumb-item>
      </el-breadcrumb>
      <HotelHead />

    </div>

    <!-- 酒店筛选组件 -->
    <div class="screen">
      <Screen />

    </div>

  </div>
</template>

<script>
import HotelHead from "@/components/hotel/hotelHead2"
import Screen from '@/components/hotel/screen'
export default {
  components: {
    HotelHead,
    Screen
  }
}
</script>

<style>
.screen {
  margin-top: 50px;
  height: 80px;
}
.w {
  width: 1000px;
  margin: 0 auto;
  height: 1000px;
}
.title {
  width: 1000px;
  margin: 0 auto;
  padding-top: 20px;
}
</style>

