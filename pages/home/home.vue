<template>
  <view>
    <view class="topSearch">
      <view class="search_btn">
        搜索
      </view>
    </view>
    
    <!-- 轮播图 -->
    <swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" :circular="true">
      <swiper-item v-for="item,i in banners" :key="item.goods_id">
        <view class="swiper-item">
          <image  :src="item.image_src" mode="widthFix" @click="toGoodsDetail(item.goods_id)"></image>
        </view>
      </swiper-item>
    </swiper>
    
    <!-- 分类导航区域 -->
    <view class="nav-list">
       <view class="nav-item" v-for="(item, i) in navList" :key="i" @click="clickNav(item)">
         <image :src="item.image_src" class="nav-img"></image>
       </view>
    </view>
    
    <!-- 楼层区域 -->
    <view class="floor-list">
      <!-- 楼层 item 项 -->
      <view class="floor-item" v-for="(item, i) in floors" :key="i">
        <!-- 楼层标题 -->
        <image :src="item.floor_title.image_src" class="floor-title"></image>
        <view class="floor-img-box">
                  <!-- 左侧大图片的盒子 -->
                  <view class="left-img-box">
                    <image @click="goGoodsList(item.product_list[0].navigator_url)" :style="{width:item.product_list[0].image_width + 'rpx'}" class="left-img" :src="item.product_list[0].image_src"></image>
                  </view>
                  <!-- 右侧 4 个小图片的盒子 -->
                  <view class="right-img-box">
                    <view class="right-img-item" v-if="i !== 0" v-for="item2,i in item.product_list">
                      <image @click="goGoodsList(item2.navigator_url)" class="image":style="{width:item2.image_width + 'rpx'}" :src="item2.image_src"></image>
                    </view>
                  </view>
        </view>
      </view>

    </view>

  </view>
</template>

<script>
  import { getBanners,getNavs,getFloor} from '@/api/home.js'
  export default {
    data() {
      return {
        banners:[],
        navList:[],
        floors:[]
      };
    },
      
    onLoad(){
      this.loadBanners()
      this.getNavs()
      this.getFloor()
    },
    methods:{
      // 获取轮播图信息
     async loadBanners(){
      const res = await getBanners()
      this.banners = res.message
      },
      // 去商品页面
      toGoodsDetail(id){
        console.log(123);
        uni.navigateTo({
          url:'/subpkg/goods_detail/goods_detail?id='+id
        })
      },
     async getNavs(){
       const res = await getNavs()
       console.log(res);
       this.navList = res.message
      },
      clickNav(item){
        uni.switchTab({
          url:'/pages/cate/cate'
        })
      },
      // 获取楼层数据
     async getFloor(){
        const res = await getFloor()
        console.log(res.message);
        this.floors = res.message
      },
      goGoodsList(url){
        console.log();
        uni.navigateTo({
          url:'/subpkg/goods_list/goods_list'+ url.slice(url.indexOf('?'))
        })
      }
    }
  }
</script>

<style lang="scss">
.topSearch {
  width: 750rpx;
  height: 70px;
  background-color: #c00000;
  display: flex;
  justify-content: center;
  overflow: hidden;
  .search_btn {
    width: 600rpx;
    height: 30px;
    margin-top: 30px;
    background-color: #fff;
    border-radius: 10px;
    display: flex;
    justify-content: center;
    align-items: center;
  }
}
swiper {
 height: 330rpx;

 .swiper-item,
 image {
   width: 100%;
   height: 100%;
 }
}
.nav-list {
  display: flex;
  justify-content: space-around;
  margin: 15px 0;

  .nav-img {
    width: 128rpx;
    height: 140rpx;
  }
}
.floor-title {
    height: 60rpx;
    width: 100%;
    display: flex;
  }

  .right-img-box {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-around;

    .image {
      height: 190rpx;
    }
  }

  .floor-img-box {
    display: flex;
    padding-left: 10rpx;

    .left-img {
      height: 392rpx !important;
    }
  }
</style>