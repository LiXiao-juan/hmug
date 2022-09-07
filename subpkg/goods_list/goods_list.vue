<template>
  <view>
  <van-card
  v-for="item in goods"
  :key="item.good_id"
  :price="item.goods_price | toFixed"
    :title="item.goods_name"
    :thumb="item.goods_small_logo || defaultPic"
    :thumb-link="`/subpkg/goods_detail/goods_detail?id=${item.good_id}`"
  />
  </view>
</template>

<script>
  import { getGoodsList } from '../../api/goods.js'
  import defaultPic from '../../static/logo.jpg'
  import toast from '../../utils/toast.js'
  export default {
    data() {
      return {
        // 获取商品列表对象
        queryData:{
          query:'',
          cid:'',
          pagenum:1,
          pagesize:10
        },
        total:'',
        goods:[],
        defaultPic,
        isLoading:false
      };
    },
    onLoad({query}){
      this.queryData.query = query
      this.loadGoodsList()
    },
    onPullDownRefresh(){
      this.queryData = {
          query:this.queryData.query,
          cid:'',
          pagenum:1,
          pagesize:10
        }
        this.loadGoodsList(() => {
          uni.stopPullDownRefresh()
        })
    },
    // 上拉刷新
    onReachBottom() {
      if(this.isLoading)return
      if(this.queryData.pagenum * this.queryData.pagesize >= this.total)return toast('暂无更多数据')
      this.queryData.pagenum++
      this.loadGoodsList()
    },
    methods:{
     async loadGoodsList(cb){
       this.isLoading = true
      const {message} = await getGoodsList(this.queryData)
      this.isLoading = false
      this.total = message.total 
      this.goods = [...this.goods,...message.goods] 
      
      cb && cb()
      }
    }
  }
</script>

<style lang="scss">

</style>
