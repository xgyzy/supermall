<template>
    <div class="detail">
        <detail-nav-bar class="detail-nav"></detail-nav-bar>
        <scroll class="content" ref="scroll">
            <detail-swiper :top-images="topImages" ></detail-swiper>
            <detail-base-info :goods="goods"></detail-base-info>
            <detail-shop-info :shop="shop"></detail-shop-info>
            <detail-goods-info :detail-info="detailInfo" @imageLoad='imageLoad'></detail-goods-info>
            <detail-param-info :paramInfo='paramInfo'></detail-param-info>
        </scroll>

    </div>
</template>
<script>
import DetailSwiper from './childComps/DetailSwiper'
import DetailNavBar from './childComps/DetailNavBar'
import DetailBaseInfo from './childComps/DetailBaseInfo'
import DetailShopInfo from './childComps/DetailShopInfo'
import DetailGoodsInfo from './childComps/DetailGoodsInfo'
import DetailParamInfo from './childComps/DetailParamInfo.vue'
import {getDetail,Goods,Shop,GoodsParam} from 'network/detail'
import Scroll from 'components/common/scroll/Scroll'
export default {
    name:'Detail',
    components:{
        DetailNavBar,
        DetailSwiper,
        DetailBaseInfo,
        DetailShopInfo,
        DetailGoodsInfo,
        DetailParamInfo,
        Scroll
    },
    data() {
        return {
            iid:null,
            topImages:[],
            goods:{},
            shop:{},
            detailInfo:{},
            paramInfo:{}
        }
    },
    created(){
        this.iid=this.$route.params.iid
        getDetail(this.iid).then(res=>{
            const data=res.result
            console.log(res)
            this.topImages=data.itemInfo.topImages
            this.goods=new Goods (data.itemInfo,data.columns,data.shopInfo.services)
            // console.log(this.goods)
            this.shop=new Shop(data.shopInfo)
            console.log(this.shop)
            this.detailInfo=data.detailInfo
            console.log(this.detailInfo)
            this.paramInfo=new GoodsParam (data.itemParams.info,data.itemParams.rule)
            console.log(this.paramInfo)
        })
    },
    methods:{
        imageLoad(){
            this.$refs.scroll.refresh()
        }
    }
}
</script>
<style scoped>
.detail{
    background-color: #fff;
    z-index: 9;
    height: 100vh;
}
.detail-nav{
    position: relative;
    z-index: 9;
    background-color: #fff;
}
.content{
    height: calc(100% - 44px - 2.69rem);
    overflow: hidden;
}
</style>